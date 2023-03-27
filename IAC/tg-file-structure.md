# File Structure

There are multiple strategies out there for structuring your Terraform code. And I would recommend going for something that is sane for the number of resources that you have, the number of collaborator and your release cycle.

## Structure 1. One Env to Rule Them All

```
.
├── infrastructure/
│   ├── main/
│   │   ├── main.tf
│   │   ├── <resource>.tf
│   │   └── variables.tf
│   │
│   └── variables/
│       ├── dev/
│       │   └── terragrunt.hcl
│       └── prod/
│           └── terragrunt.hcl
│
├── scripts/
│   └── install_terra_env
│
├── .gitignore
├── .terraform-version
├── .terragrunt-version
├── Makefile
└── README.md
```

The DRY Terraform code is located in `main/`, in which `main.tf` list the providers and initialize the backend ; `variables.tf` define all the variables that are going to be provided either by env vars or by Terragrunt ; and as many `<resource>.tf` file as you need, like `s3.tf` or `lambda.tf`...

Then the Terragrunt configuration is located in `variables/`, subdivided per environement. Every `terragrunt.hcl` provides the overrides for backend configuration and the variables to pass Terraform.

This structure is simpler, Terragrunt is only used to replace a `.tfvars` file, and basically nothing else. There is only one state file per env, and when applying changes, it will apply all the changes in the env. Handling multiple regions require to use aliased providers and lot of conditionals.

## Structure 2. There is a Module for That

```
.
├── infrastructure/
│   ├── modules/
│   │   ├── <module1>/
│   │   │   ├── main.tf
│   │   │   ├── data.tf
│   │   │   ├── outputs.tf
│   │   │   └── vars.tf
│   │   └── <module2>/
│   │       ├── main.tf
│   │       ├── data.tf
│   │       ├── outputs.tf
│   │       └── vars.tf
│   │
│   └── config/
│       ├── terragrunt.hcl
│       ├── <account>/
│       │   ├── account.hcl
│       │   └── <region>/
│       │       ├── region.hcl
│       │       └── <env>/
│       │           ├── env.hcl
│       │           └── module1/
│       │           │   └── terragrunt.hcl
│       │           └── module2/
│       │               └── terragrunt.hcl
│       └── prod/
│           ├── account.hcl
│           └── us-east-1/
│               ├── region.hcl
│               └── prod/
│                   ├── env.hcl
│                   └── module1/
│                   │   └── terragrunt.hcl
│                   └── module2/
│                       └── terragrunt.hcl
│
├── scripts/
│   └── install_terra_env
│
├── .gitignore
├── .terraform-version
├── .terragrunt-version
├── Makefile
└── README.md
```

The DRY code is split into functional units - or modules - that all have `main.tf` to specify the version requirements and create the resources, `vars.tf` to define the expected inputs, `outputs.tf` to define the module outputs. If needed, more `.tf` files are created to split the resources in a more manageable way.

Terragrunt configuration is split in a `<account>` - `<region>` - `<env>` - `<module>` manner. Allowing us to apply the changes for a whole account, a whole region, a whole env, or only a module. At the root of `config/` we can find a `terragrunt.hcl` which on its own cannot work - it's a template that will be included by the modules later on.

That template dynamically grabs the local variables defined on the env, region and account levels. It will generate a `provider.tf` and a `backend.tf` based on those variables. The terraform state is created per module, and follows the relative path to that module on the backend to avoid confusion.

The `account.hcl`, `region.hcl` and `env.hcl` configuration files only define variables.

Finally, at the deepest level, we create a folder for every module we will use and create a `terragrunt.hcl` file inside. That configuration file will dynamically include the template defined at the root. We can define dependencies with other modules, listing the outputs we need - Terragrunt will obtain the outputs we need before running this specific module. For the Terraform code to apply, we can point to a module in the `modules/` folder, or we can point to a remote module.

This structure has more complexity at setup, as there are more levels involved and writing modules for everything add steps in the work.  
However, it does bring really strong advantages:
- separation of state files reduces the risk when moving resources around and follows the separation of concerns principle
- applying a full env can take faster to run as Terragrunt will run some modules in parallel if possible
- increase reusability of terraform code
- allow only localized changes to be applied without taking the risk to apply any other changes on the rest of the resources
- allow to handle resources over multiple regions in a sane manner 
- allow greater/safer collaboration on the code itself
