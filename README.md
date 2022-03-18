# terraform-cloudprovider-cli

Build Docker images with various cloud provider CLIs.  Everyone in your team can now have a consistent experience working with your cloud provider.  Additionally each image can be used as a deployment container as well which allows you to test locally before running it in a CI/CD pipeline.  Saving time and money for all.

☁️ Cloud providers supported in the various container images! ☁️

* [AWS](./terraform-aws-go-cli)
* [Azure](./terraform-azure-go-cli)
* [GCP](./terraform-gcloud-go-cli)
* [All of the above](./terraform-multicloud-go-cli)

Author: [Lim Chooi Guan](https://www.linkedin.com/in/cgl88/) (AWS Architect, [AWS Certified Solutions Architect](https://www.credly.com/badges/c54918d6-6370-4099-afa8-122d6d4fa067))

## Pre-requisites 🛠

Required if you wish to build each image locally.

* Docker  
* Cloud provider account 

## Already have Docker?

The images exist on Dockerhub.  You can run them directly in your terminal.

#### AWS
For ARM processors, run
```
docker run -it minimice/terraform-aws-go-cli-aarch64:latest
```
For x86 processors, run
```
docker run -it minimice/terraform-aws-go-cli-x86_64:latest
```
#### Azure
For x86 processors, run
```
docker run -it minimice/terraform-azure-go-cli-x86_64:latest
```
#### GCP
For ARM processors, run
```
docker run -it minimice/terraform-gcloud-go-cli-aarch64:latest
```
For x86 processors, run
```
docker run -it minimice/terraform-gcloud-go-cli-x86_64:latest
```
#### Multi-cloud
For ARM processors, run
```
docker run -it minimice/terraform-multicloud-go-cli-aarch64:latest
```
For x86 processors, run
```
docker run -it minimice/terraform-multicloud-go-cli-x86_64:latest
```