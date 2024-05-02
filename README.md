# sysdig-cloudwatch-terraform
Terraform module for configuring the Sysdig CloudWatch Metric Stream integration

## Example Usage
```hcl
provider "aws" {
  region = "us-west-2"
}

module "sysdig_cloudwatch_stream" {
  source = "github.com/patrickeasters/sysdig-cloudwatch-terraform"

  sysdig_monitor_url       = "https://us2.app.sysdig.com"
  sysdig_monitor_api_token = "xxxxxxxxx-xxxxx-xxxxxxxxx"
}

output "cloudwatch_role_name" {
  value = module.sysdig_cloudwatch_stream.aws_iam_role_name
}

output "cloudwatch_account_id" {
  value = module.sysdig_cloudwatch_stream.aws_account_id
}
```

## Configuring in Sysdig
1. After applying the module against your AWS account, note the outputs of `aws_iam_role_name` and `aws_account_id`.
2. From the Sysdig Monitor UI, navigate to **Integrations** > **Cloud Accounts**/
3. Click **Add Account** and choose **AWS**.
4. Select **Cloud Watch Monitoring** and **New Account** from the modal, and click **Next**.
5. Click **CloudWatch Metric Streams**, then click the link labeled **Configure Manually**.
6. Select the **Role Delegation** option, then enter the **Account Id** and **Role** given from the Terraform output.
7. Click **Confirm** and wait ~10-15 minutes to confirm metric data is being ingested properly.

<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | n/a |
| <a name="provider_http"></a> [http](#provider\_http) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_cloudwatch_metric_stream.stream](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_stream) | resource |
| [aws_iam_role.cloudwatch_to_firehose](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.firehose_to_s3](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role.sysdig_to_cloudwatch](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy.cloudwatch_read](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy.firehose_read_s3](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy.kinesis_write](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_kinesis_firehose_delivery_stream.stream](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_firehose_delivery_stream) | resource |
| [aws_s3_bucket.stream_fallback](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [http_http.sysdig_cloud_info](https://registry.terraform.io/providers/hashicorp/http/latest/docs/data-sources/http) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_sysdig_monitor_api_token"></a> [sysdig\_monitor\_api\_token](#input\_sysdig\_monitor\_api\_token) | Sysdig Monitor API token for an admin user | `string` | n/a | yes |
| <a name="input_sysdig_monitor_url"></a> [sysdig\_monitor\_url](#input\_sysdig\_monitor\_url) | Sysdig Monitor URL (e.g. https://us2.app.sysdig.com ) | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_aws_account_id"></a> [aws\_account\_id](#output\_aws\_account\_id) | Current AWS account ID (needed to configure integration) |
| <a name="output_aws_iam_role_name"></a> [aws\_iam\_role\_name](#output\_aws\_iam\_role\_name) | Name of the IAM role for Sysdig's SaaS to assume (needed to configure integration) |
<!-- END_TF_DOCS -->