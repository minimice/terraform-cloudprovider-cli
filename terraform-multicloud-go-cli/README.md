# terraform-multicloud-go-cli

This Dockerfile builds a cli where you can run terraform, gcloud, aws, azure and go commands. Combines all the major cloud provider CLIs into one handy container image.  

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠
* Docker  
* Google/AWS/Azure account

## Quick start 🍕
1. Clone this repo.
2. For x86 processors, open up a terminal window to this directory and run:  
   `docker build -f 'Dockerfile_x86_64' -t local/terraform-multicloud-go-cli .`  
   For ARM processors, open up a terminal window to this directory and run:  
   `NO SOLUTION YET - TBD`
3. Open up a terminal to your terraform module folder, now run the container using the new image:

    `docker run --rm --name terraform-multicloud-go-cli -v $(pwd):/workspace -it local/terraform-multicloud-go-cli bash`

4. In the container, test that your cloud CLIs works by running:
   ```
   gcloud --version
   az version
   aws --version
   ```

5. Done! Have a coffee! ☕️