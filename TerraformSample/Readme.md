# Creating a Terraform to deploy in Azure provider
- Introduction.
    This demonstrates how to provision infrastructure in Azure using Terraform
    Create Terrafrom Configuration -> contains 3 key files under the workspace(folder).
    - main.tf - contains the declarative configuration
      - provider section
      - data secion
      - resource section
    - variables.tf - best practice to separte the variables from main. You may have default values which you can set and you can easily override across multiple environments.
    - outputs.tf - is useful to output information you don't know before deployment. like the ip-address once it's deployed. 
   
    Terraform components
        - **Terraform core** - is responsible for number of things including state management. 
        - **Modules** - are small, resuable Terraform configurations that let you manage a group of related resources as if they were a single resource. 
        - **Providers** - are the plugins that Terraform uses to manage those resources. Every supported service or infrastructure platform has a provider that defines which resources are available and performs API calls to manage those resources.
        - **The Terraform Registry** makes it easy to use any provider or module. To use a provider or module from this registry, just add it to your configuration; when you run `terraform init`, Terraform will automatically download everything it needs.
    
    Terraform State
       - Managed by Terraform core
       - Terraform stores the state of managed resources
       - Provides teh mapping of resource reality to your resource desired configuration 
       - State files also track metadata 
       - state is stored in local terrafrom.tfstate file which should not be manually edited 
       - when update is issued, backup will also be created. 
       - As best practice resources managed by terraform should not be modified outside of Terraform.
    
    Key Points
    -  Terraform Use Declarative language 
    -  Resources use the same format no matter which provider 
    -  `
           resource "type" "name" {
                  parameter1 = "value"
                  parameter2 = "value"
           }
    -  `
    -  
## Prerequisites
      - Azure CLI - [install](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli). 
      - Copy Terraform in local PC and register the path in Environment variable 
      - VS Code and Terraform extension. 


## Build and Run the sample
        - Clone the folder
        - on the Terminal, Az login
          - Make sure to be on the right directory
            -  az login --tenant xxx --allow-no-subscriptions
            -  az account list -o table (verify it's enabled)
       -  Terraform fmt (this command formats the tf files, incase the formating is wrong)
       -  Terraform init 
          -  this command initialize terrafrom. By taking the workspace, it parse through them and creates initial state.
          -  if you are using plugins like Azure plugins, it automatically downloads in .terraform folder name.
       -  Terraform plan 
          -  This command view the changes that will be applied and create the execution plan.
          -  if it's not the first time when you are running it then it compares it against the state file. 
          -  You will be able to view, what's going to be created/modified as part of the summery output.
       -  Terraform apply
          -  Apply the changes, take the desired state configruation and apply it on the actual environment (Azure).
          -  You can skip the approval by adding auto-approval, "terraform apply -auto-approve"
          -  Once you executed it, you will notice terraform.tfstate file which holds the state.
          -  you can do "terraform show" to see the detail of the state file. 
       -  Clean up resources
          -  terraform destroy
  
## Helpful commands
       - terrafrom show -> human readable view of the state file. 
       - Show specific resource from state file
         - terraform state list
         - terraform state show azurerm_storage_account.StorAccount1
         - terraform state show azurerm_storage_container.Container1
       - How to update using command line prameter 
         - terraform plan -var 'replicationType=GRS'
         - terraform apply -var 'replicationType=GRS' -auto-approve
       - To visually see the state
         - terraform graph > base.dot
         - terraform graph |dot -Tsvg > graph.svg 
           - you can use [graphviz](https://graphviz.gitlab.io/download/) to view 
       - If resources changed outside of terraform and state not current
         - terraform refresh
  
## Reference
    * [Terraform](https://registry.terraform.io/)
    * [List of providers](https://registry.terraform.io/browse/providers)
    * [List of Terraform Modules](https://registry.terraform.io/browse/modules)
    * [Azure Terraform quickstart projects](https://github.com/Azure/terraform/tree/master/quickstart)