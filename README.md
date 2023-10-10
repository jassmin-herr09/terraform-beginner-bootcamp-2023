# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging. [semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Steps
### Install the Terraform CLI 

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed refer to the latest install CLI instructions via Terraform Documentation and chnage the scripting for install. 
[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Consideratins for Linux Distribution 
This project is built agains Ubuntu.
Please consider checking your Linux Distribution and change accordingly to distribution needs
[How to check OS version in Linux ](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS version:

```$cat /etc/os-release```

```
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts
While fixing the Terraform CLI gpg deprecation issues, we noticed that bash scripts were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI.

This bash is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task file([.gitpod.yml](.gitpod.yml)) tidy. 
- This will allow us to easily debug and execute manually the Terraform CLI install 
- This will allow better portability for other projects that need to install Terraform CLI. 


### Shebang

A Shebang tells the bash script what program that will interpret the script. eg. `#!/bin/bash`


ChatGPT recommended we use this format for bash:
`#!/bin/bash`

- for portability for different OS distributions
- will search user's PATH for the bash executable

(More about the Shebang here )[https://en.wikipedia.org/wiki/Shebang_(Unix)]

## Execution Considerations

When executing the bash script we can use the `./` shorthand notation to execute the bash script.

eg. `source ./bin/install_terraform_cli`


#### Linux Permissions Considerations

Linux permissions works as follows:
In order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode. 

```
chmod u+x ./bin/install_terraform_cli
```
Alternatively:
```sh
chmod 744 ./bin/install_terraform_cli
```
[More abouot chmod here](https://en.wikipedia.org/wiki/Chmod)

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not refurn if we restart an existing workspace.
[Gitpod workspace docs here](https://www.gitpod.io/docs/configure/workspaces/tasks)

### Working Env Vars

#### env command

We can list out all Envirornmnt Variables( Env Vars ) using the `env` command.

We can filter specific env vars using eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO = 'world'`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when running a command

```sh
HELLO='world' ./bin/print_message
```
withing a bash script we can set env var without writing export eg.

```sh
#!/usr/bin/env bash
HELLO='world'

echo $HELLO
```

#### Printing Vars
We can print an Env Var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in VSCode it will not be aware of any env vars that you have set in another
window.
If you want Env Vars to persist across all future terminals, that are open. you need to set env vars in bash profiles.

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals open in those
workspaces
You can also set en vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for this project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting started Install(AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command:

```sh
aws sts get-caller-identity
```
If it is successful you should see a json payload that looks like this:

```json
{
    "UserId": "WSAFHCMVAEXAMPLEHERE12345",
    "Account": "1234567891012",
    "Arn": "arn:aws:iam::12345678910:user/terraform-beginner-bootcamp"
}

```

Well need to generate AWS CLI creds from IAM User in order to the user AWS CLI.

## Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform registr which located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an interface to APIs that will allow you to create resources in terraform.
- **Modules** are a way to make large amounts of terraform code modular,portable and sharable.

## Terraform Console

We can see a list of all the Terraform commands by simply typing `terraform`

#### Terraform Init

At the start of a new terraform project we will run `terraform init` to download the binaries for the terraform providers that we will use in this project. 

#### Terraform Plan

This will generate out a changeset, about the state of our infrastructure and what will be changed. 
We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting. 

#### Terraform Apply
This will run a plan and pass the changeset to be executed by terraform, apply should prompt us yes or no.

If we want to automatically approve an apply we can provide the auto approve flat eg. `terraform apply --auto-approve`


#### Terraform Destroy 

`terraform destroy`

This will destroy resources.
You can also use the auto approve flag to skip the approve prompt. eg `terraform apply --auto-approve`

#### Terraform Lock Files
`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project. 

The Terraform Lock Files **should be committed** to your Version Control System eg Github


#### Terraform State Files

`.terraform.tfstate` contain informatin about the current state of your infrastructure. 
This file **should not be committed**

This file can contain sensitive data. 
If you lose this file, you can lose knowing the state of your infrastructure. 

`.terraform.tfstate.backup` is the previous state file state.

#### Terraform Directory 

`.terraform` directory contains binaries of terraform providers. 


#### Terraform & S3

Had to follow the S3 rules and set by Amazon, as shown [here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html)