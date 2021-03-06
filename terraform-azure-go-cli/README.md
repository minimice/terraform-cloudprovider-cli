# terraform-azure-go-cli

✅ Multi-platform ready.  Currently only supporting x86_64 as Microsoft has not made an ARM64 version ready yet.

This Dockerfile builds a cli where you can run terraform, azure, and go commands.  You can use Azure service principle credentials (or your own credentials) by passing in the following required environment variables:
- ARM_TENANT_ID
- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- AZURE_TENANT_ID (set this to the same value as ARM_TENANT_ID)
- AZURE_CLIENT_ID (set this to the same value as ARM_CLIENT_ID)
- AZURE_CLIENT_SECRET (set this to the same value as ARM_CLIENT_SECRET)

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* Azure account with service principle 

## Pre-installed tools
- `az`, the Azure CLI (X86_64 only)
- `terraform`
- `terratest_log_parser`
- `go`

## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:
   `docker build -t local/terraform-azure-go-cli:latest .`
   For ARM processors, open up a terminal window to this directory and run:  
   `NO SOLUTION YET - TBD`
   See https://github.com/Azure/azure-cli/issues/7368
3. To run the image using a service principle, e.g. the service principle called `devops` in your subscription, you must obtain the secret. This will be used in the next step.
4. Open up a terminal to your terraform module folder then run the following command (replace `YOUR-SECRET-HERE` with the secret value obtained from the previous step) and pass in all your environment variables by running the following command:

    `docker run --env ARM_TENANT_ID="1234567-9999-1234-6666-61bf24ebbed8" --env ARM_SUBSCRIPTION_ID="1111111-2222-3333-ac5a-54e40da6d478" --env ARM_CLIENT_ID="11111111-2222-3333-4444-ae62873954cc" --env ARM_CLIENT_SECRET="YOUR-SECRET-HERE" --env AZURE_CLIENT_ID="11111111-2222-3333-4444-ae62873954cc" --env AZURE_CLIENT_SECRET="YOUR-SECRET-HERE" --env AZURE_TENANT_ID="1234567-9999-1234-6666-61bf24ebbed8" --rm --name terraform-azure-go-cli -v $(pwd):/workspace -it local/terraform-azure-go-cli bash`

5. In the container, test that the CLI is installed by running:
   `az version`

6. (Optional) To build specific platform versions, and push to your own Dockerhub registry, run:
   ```
   `docker buildx build --platform linux/amd64,linux/arm64 -t YOUR_DOCKERHUB_ID/terraform-azure-go-cli:latest --push .`
   ```

7. Done! Have a coffee! ☕️
