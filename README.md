# In Development.......
# Terraform script to create Security Control Policies - SCP





```hcl
In Details this script will create a SCP who doesn't allow accounts leave Organization.

```

```hcl
## Check out the script


## Provider config
provider "aws" {
    region  = "us-east-1"
    profile = "payer"
}

terraform {
  backend "s3" {
    profile                     = "lab"
    bucket                      = "meu-curso-aws-terraform-remote-state-dev"
    key                         = "scp/scp.tfstate"
    region                      = "us-east-1"
    encrypt                     = true
#    skip_requesting_account_id  = true
#    skip_credentials_validation = true
#    skip_get_ec2_platforms      = true
#    skip_metadata_api_check     = true
  }
}

#module "scp_prevent_leave_orgs" {
#    source = "./terraform-aws-organizations-policy/"

resource "aws_organizations_policy" "preventLeaveorganizations" {

    name            = "Senior-PreventLeaveOrganizations"
    description     = "SCP Senior Sistemas - Prevent Leave Organizations"
    type            = "SERVICE_CONTROL_POLICY"

    # Policy
    content  = <<CONTENT
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PreventLeaveOrganizations",
      "Effect": "Deny",
      "Action": "organizations:LeaveOrganization",
      "Resource": "*",
      "Condition": {
        "ArnNotLike": {
          "aws:PrincipalArn": "arn:aws:iam::*:role/aws-reserved/sso.amazonaws.com/AWSReservedSSO_CompassoInternoAdmins*"
        }
      }
    }
  ]
}
                CONTENT

}



#SCP preventLeaveorganizations

#Organization Account
#resource "aws_organizations_policy_attachment" "account" {
#  policy_id = aws_organizations_policy.preventLeaveorganizations.id
#  target_id = "977077455949"
#}

#Organization Root

resource "aws_organizations_policy_attachment" "root" {
  policy_id = aws_organizations_policy.preventLeaveorganizations.id
  target_id = "r-xu7c"
}

#Organization Unit

#resource "aws_organizations_policy_attachment" "OU" {
#  policy_id = aws_organizations_policy.preventLeaveorganizations.id
#  target_id = "ou-xu7c-0uv7ba3r"
#}


```

## After that we can create a auto scaling Infra

- Application Load Balance
- Launch configuration
- Auto scaling Group
- Target Group

```




## Requirements
| Name | Version |
| ---- | ------- |
| aws | ~> 3.* |
| terraform | ~> v1.0.5 |
