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

### Refactoring into Bash Scripts
While fixing the Terraform CLI gpg deprecation issues, we noticed that bash scripts were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI.

This bash is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task file([.gitpod.yml](.gitpod.yml)) tidy. 
- This will allow us to easily debug and execute manually the Terraform CLI install 
- This will allow better portability for other projects that need to install Terraform CLI. 


https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/
https://en.wikipedia.org/wiki/Shebang_(Unix)

https://en.wikipedia.org/wiki/Chmod
https://www.gitpod.io/docs/configure/workspaces/tasks