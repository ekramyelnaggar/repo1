
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0.3 |
| <a name="requirement_oci"></a> [oci](#requirement\_oci) | 4.105.0 |

## Geting started
- Before runing terraform init.Please adjust the terraform.tfvar.template  and Rename it to terraform.tfvars see below 
```
vi terraform.tfvar
user_ocid        = "ocid1.user.oc1.."           #CHANGE ME
fingerprint      = "1c.."                       #CHANGE ME
private_key_path = "~/.oci/oci_api_key.pem"     #CHANGE ME
tenancy_ocid     = "ocid1.tenancy.oc1.."        #CHANGE ME
region           = "us-ashburn-1"               #CHANGE ME
```

## Note
 This stack uses a local Module to create 2 levels of subcompartments as shown in the below local section 
| Compartment      | level |
-------------------|--------
| Main (Enclosing) compartment | 1|
|Level1 Subcompartments |4|
|Level2 Subcompartments |4|
```
l1_subcomp = {  #Level 1 subcompartment     || Parent compartment => var.main_compartment_name
      comp-shared                = "for shared services like AD, Commvault,Monitoring"  
      comp-network               = "for all FW, VCNs and LBRs"
      comp-security              = "for Security related resources like Vaults, keys"
      (var.app_compartment_name) = "Parent compartment for all application resources"
    },
    l2_subcomp = { #Level 2 subcompartments  || Parent compartment => L1 var.app_compartment_name
      "${var.app_compartment_name}-prod"  = " production VMs"
      "${var.app_compartment_name}-nprod" = "non-production VMs"
      "${var.app_compartment_name}-dr"    = "DR VMs"
      "${var.app_compartment_name}-db"    = "Database instances and resources"
    }
```
## Providers

| Name | Version |
|------|---------|
| <a name="provider_oci"></a> [oci](#provider\_oci) | 4.105.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_level_1_sub_compartments"></a> [level\_1\_sub\_compartments](#module\_level\_1\_sub\_compartments) | ./modules/iam-compartment | n/a |
| <a name="module_level_2_sub_compartments"></a> [level\_2\_sub\_compartments](#module\_level\_2\_sub\_compartments) | ./modules/iam-compartment | n/a |

## Resources

| Name | Type |
|------|------|
| [oci_identity_compartment.iam_compartment_main](https://registry.terraform.io/providers/oracle/oci/4.105.0/docs/resources/identity_compartment) | resource |
| [oci_identity_compartments.app_comp](https://registry.terraform.io/providers/oracle/oci/4.105.0/docs/data-sources/identity_compartments) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_app_compartment_name"></a> [app\_compartment\_name](#input\_app\_compartment\_name) | n/a | `string` | `"comp-app"` | no |
| <a name="input_fingerprint"></a> [fingerprint](#input\_fingerprint) | n/a | `any` | n/a | yes |
| <a name="input_main_compartment_desc"></a> [main\_compartment\_desc](#input\_main\_compartment\_desc) | n/a | `string` | `"Enclosing compartment at root level"` | no |
| <a name="input_main_compartment_name"></a> [main\_compartment\_name](#input\_main\_compartment\_name) | n/a | `string` | `"mycomp"` | no |
| <a name="input_private_key_path"></a> [private\_key\_path](#input\_private\_key\_path) | n/a | `any` | n/a | yes |
| <a name="input_region"></a> [region](#input\_region) | n/a | `any` | n/a | yes |
| <a name="input_tenancy_ocid"></a> [tenancy\_ocid](#input\_tenancy\_ocid) | n/a | `any` | n/a | yes |
| <a name="input_user_ocid"></a> [user\_ocid](#input\_user\_ocid) | n/a | `any` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_comp-app-db-ocid"></a> [comp-app-db-ocid](#output\_comp-app-db-ocid) | n/a |
| <a name="output_comp-app-dr-ocid"></a> [comp-app-dr-ocid](#output\_comp-app-dr-ocid) | n/a |
| <a name="output_comp-app-nprod-ocid"></a> [comp-app-nprod-ocid](#output\_comp-app-nprod-ocid) | n/a |
| <a name="output_comp-app-ocid"></a> [comp-app-ocid](#output\_comp-app-ocid) | n/a |
| <a name="output_comp-app-prod-ocid"></a> [comp-app-prod-ocid](#output\_comp-app-prod-ocid) | n/a |
| <a name="output_comp-network-ocid"></a> [comp-network-ocid](#output\_comp-network-ocid) | n/a |
| <a name="output_comp-security-ocid"></a> [comp-security-ocid](#output\_comp-security-ocid) | n/a |
| <a name="output_comp-shared-ocid"></a> [comp-shared-ocid](#output\_comp-shared-ocid) | n/a |
| <a name="output_l1_sub_compartment"></a> [l1\_sub\_compartment](#output\_l1\_sub\_compartment) | shows level 1 App subcompartment details |
| <a name="output_l1_sub_compartments"></a> [l1\_sub\_compartments](#output\_l1\_sub\_compartments) | Shows all level one subcompartments details |
| <a name="output_l2_sub_compartments"></a> [l2\_sub\_compartments](#output\_l2\_sub\_compartments) | Shows all level one subcompartments details |
| <a name="output_main_compartment"></a> [main\_compartment](#output\_main\_compartment) | n/a |
