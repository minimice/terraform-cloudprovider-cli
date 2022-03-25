# Setup build arguments with default versions
ARG TERRAFORM_VERSION=1.1.7
ARG PYTHON_MAJOR_VERSION=3.7
ARG DEBIAN_VERSION=buster-20220316-slim
ARG ARCHITECTURE=amd64

# Download Terraform binary
FROM debian:${DEBIAN_VERSION} as terraform-cli
ARG TERRAFORM_VERSION
# platform of the build result. Eg linux/amd64, linux/arm/v7, windows/amd64
ARG TARGETPLATFORM
ARG ARCHITECTURE=${ARCHITECTURE}
ENV ARCHITECTURE $ARCHITECTURE
RUN apt-get update
RUN apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y --no-install-recommends curl=7.64.0-4+deb10u2
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
RUN apt-get install -y --no-install-recommends unzip=6.0-23+deb10u2
RUN apt-get install -y --no-install-recommends gnupg=2.2.12-1+deb10u1
RUN apt-get install -y --no-install-recommends wget
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS

# Conditional magic
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then ARCHITECTURE=arm64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_${ARCHITECTURE}.zip

RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig
# Hashicorp PGP public key, see https://www.hashicorp.com/security
RUN wget https://raw.githubusercontent.com/sethvargo/hashicorp-installer/master/hashicorp.asc
RUN gpg --import hashicorp.asc
RUN gpg --verify terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig terraform_${TERRAFORM_VERSION}_SHA256SUMS
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Conditional magic
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then ARCHITECTURE=arm64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && grep terraform_${TERRAFORM_VERSION}_linux_${ARCHITECTURE}.zip terraform_${TERRAFORM_VERSION}_SHA256SUMS | sha256sum -c - \
    && unzip -j terraform_${TERRAFORM_VERSION}_linux_${ARCHITECTURE}.zip

# Install GO and terratest log parser for testing
FROM debian:${DEBIAN_VERSION} as go
# platform of the build result. Eg linux/amd64, linux/arm/v7, windows/amd64
ARG TARGETPLATFORM
ARG ARCHITECTURE=${ARCHITECTURE}
ENV ARCHITECTURE $ARCHITECTURE
RUN apt-get update
RUN apt-get install -y --no-install-recommends ca-certificates
RUN update-ca-certificates
RUN apt-get install -y --no-install-recommends wget

# Conditional magic
# Install terratest log parser binary
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=amd64; elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then ARCHITECTURE=arm64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=arm64; else ARCHITECTURE=amd64; fi \
    && wget https://golang.org/dl/go1.16.3.linux-${ARCHITECTURE}.tar.gz \
    && rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.3.linux-${ARCHITECTURE}.tar.gz \
    && wget -O terratest_log_parser https://github.com/gruntwork-io/terratest/releases/download/v0.40.6/terratest_log_parser_linux_${ARCHITECTURE}

RUN chmod +x terratest_log_parser
RUN mv terratest_log_parser /usr/local/bin

# Build final image
FROM debian:${DEBIAN_VERSION}
LABEL maintainer="LIM, CHOOI GUAN"
ARG PYTHON_MAJOR_VERSION
# platform of the build result. Eg linux/amd64, linux/arm/v7, windows/amd64
ARG TARGETPLATFORM
ARG ARCHITECTURE=${ARCHITECTURE}
ENV ARCHITECTURE $ARCHITECTURE

RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=x86_64; elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then ARCHITECTURE=aarch64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=aarch64; else ARCHITECTURE=x86_64; fi \
    && echo ${ARCHITECTURE}

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates \
    git=1:2.20.1-2+deb10u3 \
    python3=${PYTHON_MAJOR_VERSION}.3-1 \
    python3-distutils=${PYTHON_MAJOR_VERSION}.3-1 \
    unzip curl less groff \
    python3-pip python3-setuptools \
    nodejs npm \
  # Needed for go terraform tests
  && apt-get install -y gcc=4:8.3.0-1 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && update-alternatives --install /usr/bin/python python /usr/bin/python${PYTHON_MAJOR_VERSION} 1 \
  && update-ca-certificates \
  # Conditional magic - install aws cli (for ARM/X86_64)
  && if [ "$TARGETPLATFORM" = "linux/amd64" ]; then ARCHITECTURE=x86_64; elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then ARCHITECTURE=aarch64; elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCHITECTURE=aarch64; else ARCHITECTURE=x86_64; fi \
  && curl --insecure "https://awscli.amazonaws.com/awscli-exe-linux-${ARCHITECTURE}.zip" -o "awscliv2.zip" \
  && echo ${ARCHITECTURE} \
  && unzip awscliv2.zip && \
    ./aws/install && \
    rm awscliv2.zip \
  # Install cfn-dia diagramming tool (for visualising Cloudformation)
  && npm i -g @mhlabs/cfn-diagram \
  # Install aws-shell
  && pip3 install aws-shell

COPY --from=terraform-cli /terraform /usr/local/bin/terraform
COPY --from=go /usr/local/go /usr/local/go
COPY --from=go /usr/local/bin/terratest_log_parser /usr/local/bin/terratest_log_parser
ENV PATH="${PATH}:/usr/local/go/bin"

WORKDIR /workspace
RUN groupadd --gid 1001 nonroot \
  && useradd --gid nonroot --create-home --uid 1001 nonroot \
  && chown nonroot:nonroot /workspace
USER nonroot

CMD ["bash"]
