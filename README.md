# terraform-cloudprovider-cli

Build Docker images with various cloud provider CLIs and other required tools.  

✅ No need to ever install any package locally on your machine.  
✅ Everyone in a team can now have a consistent experience working with your cloud provider.  
✅ Can be used as a deployment container, allowing you to test locally before running it in a CI/CD pipeline.  
✅ Pipeline agnostic.  Whatever pipeline you use, this can be deployed with consistent behaviour.  
✅ Saving ⏰  and 💰 for all.  Life is precious, don't spend time doing trivial tasks.  

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

✅ Supports multi-plaforms (x86_64, arm64)

Run
```
docker run -it minimice/terraform-aws-go-cli:latest
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