# Getting Started with Terraform

With Terraform, you can create and manage your Infrastructure as Code (IaC), allowing you to use configuration files to define infrastructure instead of using a graphical user interface. You can define, deploy, change, manage, and destroy infrastructure in a consistent and repeatable way with resource configurations that can be reused, shared, and edited by others.

In this tutorial, you will learn about Docker, install Terraform on your local system, write a configuration file to define your container and image details, initialize a new local workspace, validate it, and then destroy your deployed infrastructure.

## Prerequisites

Before you begin, you need Docker installed on your local system. Docker is an open-source platform that lets you deploy containers, which are portable images that run in almost any environment like virtual machines (VMs). But unlike VMs, containers all share the same host operating system's kernel. This makes them smaller, more efficient, and more secure than virtual machines.

### Install Docker

To install Docker, visit [Docker.com](https://www.docker.com/) and download the appropriate pre-compiled executable for your system. There are many ways to install Docker including using the command-line interface (CLI), but for this tutorial we recommend using an executable package to install the default Docker environment with no additional configuration.

### Verify Docker installation

Before proceeding further, verify that Docker is operating correctly by issuing the `docker -v` command. If Docker is running, it will respond with a version and build number.

```shell
$ docker -v
Docker version 29.3.1, build c2be9cc
```

## Install Terraform

To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and download the appropriate pre-compiled executable for your system.

Use this link to [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) and configure Terraform for your platform. Verify that the installation is working with the `terraform -version` command.

```terraform
$ terraform -version
Terraform v1.14.8
```

With Terraform installed, you can now deploy infrastructure as code.

## Configure Terraform

Before configuration, we recommend creating a new directory on your local machine to deploy infrastructure with Terraform. This directory will store a configuration file that you will populate with details for a local Docker container deployment.

```shell
$ mkdir terraform-demo
```

Change folders into the directory you just created.

```shell
$ cd terraform-demo
```

Create a file to contain your Terraform configuration code.

```shell
$ touch main.tf
```

### Add container and image details

Using a text editor, paste the following lines into the `main.tf` file to define the _kreuzwerker/docker_ Terraform Docker Provider and _nginx_ virtual image.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

> [!NOTE]
> On Windows systems, replace the `host = "unix:///var/run/docker.sock"` line with:
> `host = "npipe:////.//pipe//docker_engine"`

### Initialize your Terraform workspace

Initialize your Terraform workspace with the `terraform init` command. Initialization downloads and installs the provider and image you defined in the `main.tf` file. Read the resulting message and verify the successful initialization.

```shell
$ terraform init
Terraform has been successfully initialized!
```

### Validate configuration

Verify that your configuration is valid using the `terraform validate` command.

```shell
$ terraform validate
Success! The configuration is valid.
```

### Provision Terraform resource

If no errors are found, provision the resource with the `terraform apply` command. Approve the plan to provision your resources by responding `yes` to the prompt.

```shell
$ terraform apply
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

The command can take a few minutes to run and display a message that the resource was created.

### Destroy the infrastructure

When you no longer need the infrastructure managed by your workspace, use Terraform to destroy it.

Destroy your infrastructure using the `terraform destroy` command. Approve Terraform's plan to destroy your resources by responding `yes` to the prompt.

```shell
$ terraform destroy
Destroy complete! Resources: 2 destroyed.
```

## Next Steps

In this tutorial, you learned how to install Docker, install the Terraform CLI, and learned how to use Terraform to define, deploy, and destroy a locally provisioned infrastructure.

You can find more information about the Terraform configuration language at the link below.

- [Configuration Language](https://developer.hashicorp.com/terraform/tutorials/configuration-language) - Learn about Terraform variables, outputs, dependencies, and other features to write more detailed Terraform configurations.

