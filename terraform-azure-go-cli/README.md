# terraform-azure-go-cli

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

## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:  
   `docker build -t local/terraform-azure-go-cli .`
3. To run the image using a service principle, e.g. the service principle called `devops` in your subscription, you must obtain the secret. This will be used in the next step.
4. Open up a terminal to your terraform module folder then run the following command (replace `YOUR-SECRET-HERE` with the secret value obtained from the previous step) and pass in all your environment variables by running the following command:

    `docker run --env ARM_TENANT_ID="1234567-9999-1234-6666-61bf24ebbed8" --env ARM_SUBSCRIPTION_ID="1111111-2222-3333-ac5a-54e40da6d478" --env ARM_CLIENT_ID="11111111-2222-3333-4444-ae62873954cc" --env ARM_CLIENT_SECRET="YOUR-SECRET-HERE" --env AZURE_CLIENT_ID="11111111-2222-3333-4444-ae62873954cc" --env AZURE_CLIENT_SECRET="YOUR-SECRET-HERE" --env AZURE_TENANT_ID="1234567-9999-1234-6666-61bf24ebbed8" --rm --name terraform-azure-go-cli -v $(pwd):/workspace -it local/terraform-azure-go-cli bash`

5. Done! Have a coffee! ☕️
