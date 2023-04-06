---
title: Terraform
---

# Terraform
https://www.terraform.io/

> Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently.

Terraform is regularly updated to improve its syntax, security and functionality. The Terraform version you are using impacts the project you are working on. Therefore managing the version you are using for a specific project is important.

## TFENV

> Terraform version manager

https://github.com/tfutils/tfenv (I recommend you use v1.0.2 as v2.0.0 has some issues at the moment)

# Terragrunt
https://terragrunt.gruntwork.io/

> Terragrunt is a thin wrapper that provides extra tools for keeping your configurations DRY, working with multiple Terraform modules, and managing remote state.

Similarly to Terraform, Terragrunt is regularly updated to follow changes and improvements added to Terraform, managing its version is equally important.

## TGENV

> Terragrunt version manager

https://github.com/cunymatthieu/tgenv

# Installation

Instead of installing directly Terraform and Terragrunt, it is recommended to use versions managers to help us stay up to date and switch context between projects.

You can follow the manual steps provided on `tfenv` and `tgenv` github to get them working locally, or you can run this shell script:
```shell
#!/bin/sh

TFV=1.0.2
TGV=1.1.0

echo Installing tfenv v$TFV
git clone -b v$TFV --depth 1 https://github.com/tfutils/tfenv.git ~/.tfenv

echo Installing tgenv v$TGV
git clone -b v$TGV --depth 1 https://github.com/01011111/tgenv.git ~/.tgenv

if [[ $SHELL = '/bin/zsh' ]];
then
  echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="$HOME/.tgenv/bin:$PATH"' >> ~/.zshrc
  echo 'Exectuables added to your path, reload your ~/.zshrc'
elif [[ $SHELL = '/bin/bash' ]];
then
  echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bashrc
  echo 'export PATH="$HOME/.tgenv/bin:$PATH"' >> ~/.bashrc
  echo 'Exectuables added to your path, reload your ~/.bashrc'
else
  echo 'Add tfenv to your path: export PATH="$HOME/.tfenv/bin:$PATH"'
  echo 'Add tgenv to your path: export PATH="$HOME/.tgenv/bin:$PATH"'
fi

echo Done
```

## Version Installation

You need to install a specific version, then tell the tool to use it. This can be done with these simple commands:

- `tfenv install 0.13.0`
- `tfenv use 0.13.0`

tgenv usage is identical (the tool is actual based off of tfenv):

- `tgenv install 0.23.34`
- `tgenv use 0.23.34`

Another way is to use a .terra*-version file in your project. You can simply put the version of Terraform you need in a .terraform-version file and the version of Terragrunt in a .terragrunt-version file. In that case you only need to run `tfenv install && tgenv install` and it will pick the version from the file. You will still need to specify the version to use, which can be dynamically done by running: `tfenv use $(cat .terraform-version) && tfenv use $(cat .terragrunt-version)`

You can refer to tfenv -h and tgenv -h the additional commands.

# Infrastructure As Code

## Providers

Terraform revolves around the use of providers to map the infrastructure as code to the correct commands to manipulate a specific resource.

Providers are versioned and updated regularly to follow new feature releases and bug fixes.

Before creating any resource, you need to declare the provider(s) you need to use:

```hcl
provider "aws" {
  version = "~> 3.3"
}
```

Terraform will use that to get the correction version of the provider and run the required commands later.

You can use multiple providers if you need to interact with different platforms, and you can also have multiple instances of the same provider (when using different regions for instances) as long as you alias them.

## State

Now, Terraform uses a state to remember which resource we have, and what their settings are. On top of that, Terraform will also query the cloud provider for the resource we want to create / update / delete.

Thanks to all those check, we have 3 sources: our infrastructure as code, the terraform state, and the resources' status.

That allows Terraform to detect changes and inform us with a detailed diff before we apply any of those changes.

In order to be able to collaborate and / or to use CI / CD to apply changes, we need to store the state somewhere else than the local machine.

For instance, to store the terraform state as a file on a s3 bucket:

```hcl
terraform {
  backend "s3" {
    bucket = "mybucket"
    key    = "path/to/my/key"
    region = "us-east-1"
  }
}
```

There are a lot of different backend types available with different functionalities and options: [https://www.terraform.io/docs/backends/types/](https://www.terraform.io/docs/backends/types/).

## Resources

Every provider offers different resources and data sources that can be used.

For instance, if you want to create a S3 bucket, you need to use the aws provider (as seen above) and then you can declare it in your terraform file(s):

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-tf-test-bucket"
  acl    = "private"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```
[https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)

### Data Sources

For most resources you can create in Terraform, the is a corresponding data source.

Those are used when you don't want to own a resource through Terraform but still need data from it.

For instance, if you need the get data about a specific object on a S3 bucket:

```hcl
data "aws_s3_bucket_object" "lambda_artefacts" {
  bucket = "some-lambda-artefacts"
  key    = "lambda-xyz.zip"
}
```

From that data source, you could obtain the `version_id` of the object without owning the object through Terraform.  
(This is a common case when managing a Lambda through Terraform, for checking if the code changed and if we need to publish a new Lambda version)

# File Structures

See [File Structure]({{ site.baseurl }}/IAC/tg-file-structure.md)

# Usage

The 4 main commands that we need are:

- init
- plan
- apply
- destroy

They are initially terraform commands, but since we use terragrunt, you'll have to go into the right folder (i.e. variables/dev/ for the dev environment) and run `terragrunt <command>` there.

## init

Init will download the providers that are needed and initialize the backend.

## plan

Plan will display a diff of what will happen if we apply the current infrastructure as code

## apply

Apply will create / update or delete the resource based on the diff between the code, the state and the resources status

## destroy

Destroy will delete all the resource owned by this terraform state

# Links
- [Learn Terraform](https://learn.hashicorp.com/terraform)
- [Terragrunt QuickStart](https://terragrunt.gruntwork.io/docs/getting-started/quick-start/)

## Terraform and AWS
- [aws provider](https://www.terraform.io/docs/providers/aws/)
- [Terraform with AWS Tutorial](https://hackernoon.com/introduction-to-aws-with-terraform-7a8daf261dc0)

## Terraform and Azure
- [azurerm provider](https://www.terraform.io/docs/providers/azurerm/)
- [Azure Introduction](https://docs.microsoft.com/en-us/learn/azure/)
- [Azure and Terraform](https://docs.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform with Azure Tutorial](https://linuxacademy.com/guide/20146-a-complete-azure-environment-with-terraform/)
