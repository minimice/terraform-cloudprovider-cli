# terraform-gcloud-go-cli

✅ Supports multi-platforms (x86_64, arm64)  

This Dockerfile builds a cli where you can run terraform, gcloud, and go commands.  

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* Google account

## Pre-installed tools
- `gcloud`, the GCloud CLI
- `terraform`
- `terratest_log_parser`
- `go`

## Quick start 🍕
1. Clone this repo.
2. Open up a terminal window to this directory and run:
   `docker build -t local/terraform-gcloud-go-cli:latest .`
3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --rm --name terraform-gcloud-go-cli -v $(pwd):/workspace -it local/terraform-gcloud-go-cli bash`

4. In the container, test that your gcloud CLI and other installed software works by running:
   ```
   gcloud --version
   go version
   terraform -v
   ```

5. (Optional) To build specific platform versions, and push to your own Dockerhub registry, run:
   ```
   `docker buildx build --platform linux/amd64,linux/arm64 -t YOUR_DOCKERHUB_ID/terraform-gcloud-go-cli:latest --push .`
   ```

6. Done! Have a coffee! ☕️