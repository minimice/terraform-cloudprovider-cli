# Setup build arguments with default versions
ARG AZURE_CLI_VERSION=2.26.1
ARG TERRAFORM_VERSION=1.1.7
ARG PYTHON_MAJOR_VERSION=3.7
ARG DEBIAN_VERSION=buster-20220316-slim

# Download Terraform binary
FROM debian:${DEBIAN_VERSION} as terraform-cli
ARG TERRAFORM_VERSION
RUN apt-get update
RUN apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y --no-install-recommends curl=7.64.0-4+deb10u2
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
RUN apt-get install -y --no-install-recommends unzip=6.0-23+deb10u2
RUN apt-get install -y --no-install-recommends gnupg=2.2.12-1+deb10u1
RUN apt-get install -y --no-install-recommends wget
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig
# Hashicorp PGP public key, see https://www.hashicorp.com/security
RUN wget https://raw.githubusercontent.com/sethvargo/hashicorp-installer/master/hashicorp.asc
RUN gpg --import hashicorp.asc
RUN gpg --verify terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig terraform_${TERRAFORM_VERSION}_SHA256SUMS
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN grep terraform_${TERRAFORM_VERSION}_linux_amd64.zip terraform_${TERRAFORM_VERSION}_SHA256SUMS | sha256sum -c -
RUN unzip -j terraform_${TERRAFORM_VERSION}_linux_amd64.zip

# Install az CLI using apt-get
FROM debian:${DEBIAN_VERSION} as azure-cli
ARG AZURE_CLI_VERSION
RUN apt-get update
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
RUN apt-get install -y curl apt-transport-https lsb-release gnupg
RUN curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
    gpg --dearmor | \
    tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
RUN AZ_REPO=$(lsb_release -cs) && \
    echo $AZ_REPO && \
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    tee /etc/apt/sources.list.d/azure-cli.list && \
    apt-get update && \
    apt-get install azure-cli=${AZURE_CLI_VERSION}-1~$AZ_REPO

# Install GO and terratest log parser for testing
FROM debian:${DEBIAN_VERSION} as go
RUN apt-get update
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
RUN apt-get install -y --no-install-recommends wget
RUN wget https://golang.org/dl/go1.16.3.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.3.linux-amd64.tar.gz
# Install terratest log parser binary
RUN wget -O terratest_log_parser https://github.com/gruntwork-io/terratest/releases/download/v0.13.13/terratest_log_parser_linux_amd64
RUN chmod +x terratest_log_parser
RUN mv terratest_log_parser /usr/local/bin

# Build final image
FROM debian:${DEBIAN_VERSION}
LABEL maintainer="LIM, CHOOI GUAN"
ARG PYTHON_MAJOR_VERSION
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates=20200601~deb10u2 \
    git=1:2.20.1-2+deb10u3 \
    python3=${PYTHON_MAJOR_VERSION}.3-1 \
    python3-distutils=${PYTHON_MAJOR_VERSION}.3-1 \
  # Needed for go terraform tests
  && apt-get install -y gcc=4:8.3.0-1 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && update-alternatives --install /usr/bin/python python /usr/bin/python${PYTHON_MAJOR_VERSION} 1
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
COPY --from=terraform-cli /terraform /usr/local/bin/terraform
COPY --from=azure-cli /usr/local/bin/az* /usr/local/bin/
COPY --from=azure-cli /usr/local/lib/python${PYTHON_MAJOR_VERSION}/dist-packages /usr/local/lib/python${PYTHON_MAJOR_VERSION}/dist-packages
COPY --from=azure-cli /usr/lib/python3/dist-packages /usr/lib/python3/dist-packages
COPY --from=go /usr/local/go /usr/local/go
COPY --from=go /usr/local/bin/terratest_log_parser /usr/local/bin/terratest_log_parser
ENV PATH="${PATH}:/usr/local/go/bin"


WORKDIR /workspace
RUN groupadd --gid 1001 nonroot \
  # user needs a home folder to store azure credentials
  && useradd --gid nonroot --create-home --uid 1001 nonroot \
  && chown nonroot:nonroot /workspace
USER nonroot

CMD ["bash"]
