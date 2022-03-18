# terraform-gcloud-go-cli

This Dockerfile builds a cli where you can run terraform, gcloud, and go commands.  

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* Google account

## Quick start 🍕
1. Clone this repo.
2. For x86 processors, open up a terminal window to this directory and run:  
   `docker build -f 'Dockerfile_x86_64' -t local/terraform-gcloud-go-cli .`  
   For ARM processors, open up a terminal window to this directory and run:  
   `docker build -f 'Dockerfile_aarch64' -t local/terraform-gcloud-go-cli .`
3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --rm --name terraform-gcloud-go-cli -v $(pwd):/workspace -it local/terraform-gcloud-go-cli bash`

4. In the container, test that your gcloud CLI works by running:
   ```
   gcloud --version
   go version
   terraform -v
   ```

5. Done! Have a coffee! ☕️