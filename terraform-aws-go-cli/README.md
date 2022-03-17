# terraform-aws-go-cli

This Dockerfile builds a cli where you can run terraform, aws cli, and go commands.  Pass in the following required environment variables:
- AWS_SECRET_ACCESS_KEY
- AWS_ACCESS_KEY_ID
- AWS_DEFAULT_REGION

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* AWS account

## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:  
   `docker build -t local/terraform-aws-go-cli .`
3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --env AWS_SECRET_ACCESS_KEY="h2gmFakeh6Jrv8nQOGpCSa+Ary5" --env AWS_ACCESS_KEY_ID="AFAKEF7TR4654HFVXX" --env AWS_DEFAULT_REGION="eu-west-1" --rm --name terraform-aws-go-cli -v $(pwd):/workspace -it local/terraform-aws-go-cli bash`

4. In the container, test that your credentials work by running:
   `aws s3 ls`

5. Done! Have a coffee! ☕️
