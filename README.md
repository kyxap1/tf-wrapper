Terraform Wrapper
=================

This project is intended to provide a wrapper around terraform for any projects where infrastructure might be duplicated in different environments. If you maintain different environments (dev, stage, prod) in separate regions, data centers, or different clouds this tool will help you maintain consistency across your infrastructure.

The Fundamentals
================
**Requirements**
you must have AWS credentials, and terraform already installed.

**Project Structure**
This project requires the following format:

```
    .
    ├── environments
    │   ├── dev
    │   │   └── dev.tf
    │   ├── environment_vars.json
    │   ├── hub
    │   │   └── hub.tf
    │   └── prod
    │       └── prod.tf
    ├── main.tf
    └── variables.tf
```

In each project shared resources and variables are placed at the top level. Environment specific resources are placed under ``./environments/<environment_name>/<any files you want>``. The file ``./environment/environment_vars.json`` stores the information about your AWS remote state.

**Package Use**

Every time you run this package you need to specify the environment and the action. the environment is the name of the folder under ``./environments`` and the action is the typical action passed to terraform (ie. plan, destroy, apply, etc.)

A sample command would be: ``tf -environment prod -action plan``

**Usage Details**
When you run this command the tf-wrapper will:
- Symlink all files under ``./environments/<environment_name>/`` into the top level directory.
- It will delete the ``.terraform/terraform.tfstate`` and ``.terraform/terraform.tfstate.backup`` files as this project requires remote state config, negating the need for local copies of state after the run has completed.
- For commands ``apply`` and ``destroy`` the resulting run state will automatically be pushed.