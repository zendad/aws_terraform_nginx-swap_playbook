# Install Nginx and configure swap on AWS ec2 using ansible and terraform

### Requirements:

- Terraform
- Ansible
- AWS access

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

### Ansible Role after Terraform Provisioning:

Once the Terraform will create all the resources over AWS, you can use the Ansible to install nginx and setup swap space.
Install Ansible and use `ansible-pull` to run the playbook for the instance. Use the script `terraform/ec2_data.sh` to run this.

### To use the provided Role:
```shell
ansible-playbook site-swap.yml
```
or use this command if you are using on multiple hosts
```shell
ansible-playbook -i hosts site-swap.yml
```


