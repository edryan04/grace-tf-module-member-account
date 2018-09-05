
# grace-tf-module-member-account [![CircleCI](https://circleci.com/gh/GSA/grace-tf-module-member-account.svg?style=svg)](https://circleci.com/gh/GSA/grace-tf-module-member-account)

Terraform module for AWS subaccounts that is used in GRACE. This module is a supporting module that is required for [grace-core](https://github.com/GSA/grace-core) but may be used independently by other projects if required.

## Contributors

This module was created by the GRACE DevSecOps team. The master branch is maintained by this same group of contributors. For information on contributing, see [CONTRIBUTING.md](https://github.com/gsa/grace-tf-member-account/CONTRIBUTING.md)

## License

The license for this software is documented in [LICENSE.md](https://github.com/gsa/grace-tf-module-member-account/LICENSE.md)

## Instructions

Use Terraform to call this module in your code. Below is an example to create a member account for a new GRACE tenant named "tenant-1-prod":

    ```hcl
    module "tenant_1_prod" {
        source = "github.com/gsa/grace-tf-module-member-account/terraform/modules/member_account"

        name                        = "tenant-1-prod"
        email                       = "tenant.owner+tenant1prod@gsa.gov"
        authlanding_prod_account_id = "${module.authlanding_prod.account_id}"
        create_iam_roles            = "true"

        tenant_admin_iam_role_list     = ["${local.tenant_admin_iam_role_list}"]
        tenant_poweruser_iam_role_list = ["${local.tenant_poweruser_iam_role_list}"]
        tenant_viewonly_iam_role_list  = ["${local.tenant_viewonly_iam_role_list}"]
    }
    ```

### Required variables

You must supply the following variables to the module:

* name: The name of the account.  Must be unique within GRACE AWS Organization.
* email: Email address of the tenant owner (will be assigned to the root account).
* authlanding_prod_account_id: This is the subaccount ID of the GRACE authlanding AWS account. The authlanding account hosts IAM accounts with access to the platform. If you're using this module in a setup outside of GRACE, you should consider consulting the [grace-core repo](https://github.com/gsa/grace-core) for more information.
* create_iam_roles: Boolean that defines whether or not the default IAM roles should be created. This will be forced to "true" in a future revision.
* tenant_admin_iam_role_list: The SSM parameter that corresponds to the admin users in authlanding. Consult [grace-core repo](https://github.com/gsa/grace-core) for more information.
* tenant_poweruser_iam_role_list: The SSM parameter that corresponds to the powerusers in authlanding. Consult [grace-core repo](https://github.com/gsa/grace-core) for more information.
* tenant_viewonly_iam_role_list: The SSM parameter that corresponds to the viewonly users in authlanding.  Consult [grace-core repo](https://github.com/gsa/grace-core) for more information.

### Authors

[Jason G. Miller](mailto:jasong.miller@gsa.gov)

#### Developer / Company / Employer

Developed by SAIC, Inc. under contract GS00Q09BGD0048 for General Services Administration Information Technology (GSAIT)
