# terraform-multicloud-go-cli

This Dockerfile builds a cli where you can run terraform, gcloud, aws, azure and go commands. Combines all the major cloud provider CLIs into one handy container image.  

✅ Supports multi-platforms (x86_64, arm64)  

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* Google/AWS/Azure account

## Pre-installed tools
- `aws-shell`, run aws commands with auto-complete!  See [here](https://github.com/awslabs/aws-shell)
- `cfn-dia`, diagramming tool to visualise Cloudformation templates!  See [here](https://github.com/mhlabs/cfn-diagram)
- `aws`, the AWS CLI
- `gcloud`, the GCloud CLI
- `az`, the Azure CLI (X86_64 only)
- `terraform`
- `terratest_log_parser`
- `go`

## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:
   `docker build -t local/terraform-multicloud-go-cli:latest .`
   Note (for ARM processors): **Excludes AZ CLI as no ARM version yet for Debian**  
3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --rm --name terraform-multicloud-go-cli -v $(pwd):/workspace -it local/terraform-multicloud-go-cli bash`

4. In the container, test that your cloud CLIs and installed software work by running:
   ```
   gcloud --version
   az version
   aws --version
   go version
   terraform -v
   ```

5. (Optional) To build specific platform versions, and push to your own Dockerhub registry, run:
   ```
   `docker buildx build --platform linux/amd64,linux/arm64 -t YOUR_DOCKERHUB_ID/terraform-multicloud-go-cli:latest --push .`
   ```

6. Done! Have a coffee! ☕️