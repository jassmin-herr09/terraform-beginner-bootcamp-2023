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
You can also set en vars in the `.gitpod.yml` but this can only contain non-sensitive env vars
