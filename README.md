# Setup AWS ec2 instances using terraform

### Requirements:

- Terraform
- AWS access

##Installing terraform

Go to `https://terraform.io/downloads.html` and select the package corresponding to your OS.
`$ wget https://releases.hashicorp.com/terraform/0.6.6/terraform_0.6.6_linux_amd64.zip`
`$ unzip terraform_0.6.6_linux_amd64.zip -d whatever.directory.you.wish`

Add the directory where the unzipped contents are located to your PATH variable.

In Unix-based systems, this can be edited by opening ` ~/.bashrc` using your favorite editor, and adding the line `PATH=$PATH:<filepath>.`
In Windows, this is done by opening up Windows Explorer, right clicking on This PC, and selecting Properties -> Advanced System Settings -> Environment Variables. Select PATH and choose edit. Append the directory to the end of the path variable WITHOUT spaces.
Reload or open a new terminal so the update to your PATH is recognized.

Last, verify your install was successful:
`$ terraform -v`

### Tools Used:
Before using the terraform, we need to export `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as environment variables:

```
export AWS_ACCESS_KEY_ID="xxxxxxxxxxxxxxxx"
export AWS_SECRET_ACCESS_KEY="yyyyyyyyyyyyyyyyyyyy"
```
To Generate and show an execution plan (dry run):
```
terraform plan
```
To Builds or makes actual changes in infrastructure:
```
terraform apply
```
To inspect Terraform state or plan:
```
terraform show
```
To destroy Terraform-managed infrastructure:
```
terraform destroy
```
**Note**: Terraform stores the state of the managed infrastructure from the last time Terraform was run. Terraform uses the state to create plans and make changes to the infrastructure.


