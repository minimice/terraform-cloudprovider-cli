# terraform-aws-go-cli

✅ Supports multi-platforms (x86_64, arm64)  

This Dockerfile builds a cli where you can run terraform, aws cli, and go commands.  Pass in the following required environment variables:
- AWS_SECRET_ACCESS_KEY
- AWS_ACCESS_KEY_ID
- AWS_DEFAULT_REGION

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* AWS account

## Pre-installed tools
- `aws-shell`, run aws commands with auto-complete!  See [here](https://github.com/awslabs/aws-shell)
- `cfn-dia`, diagramming tool to visualise Cloudformation templates!  See [here](https://github.com/mhlabs/cfn-diagram)
- `aws`, the AWS CLI
- `terraform`
- `terratest_log_parser`
- `go`
## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:
   `docker build -t local/terraform-aws-go-cli:latest .`

3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --env AWS_SECRET_ACCESS_KEY="h2gmFakeh6Jrv8nQOGpCSa+Ary5" --env AWS_ACCESS_KEY_ID="AFAKEF7TR4654HFVXX" --env AWS_DEFAULT_REGION="eu-west-1" --rm --name terraform-aws-go-cli -v $(pwd):/workspace -it local/terraform-aws-go-cli bash`

4. If you're using aws-vault, you can export the env variables as follows:

    `docker run --env-file <(aws-vault exec YOUR_AWS_VAULT_PROFILE_HERE -- env | grep ^AWS_) --rm --name terraform-aws-go-cli -v $(pwd):/workspace -it local/terraform-aws-go-cli bash`

5. In the container, test that your credentials work and other installed software works by running:
   ```
   aws s3 ls
   go version
   terraform -v
   ```

6. (Optional) To build specific platform versions, and push to your own Dockerhub registry, run:
   ```
   `docker buildx build --platform linux/amd64,linux/arm64 -t YOUR_DOCKERHUB_ID/terraform-aws-go-cli:latest --push .`
   ```

7. Done! Have a coffee! ☕️
