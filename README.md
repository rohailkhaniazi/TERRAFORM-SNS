
## Usage


**IMPORTANT:** We do not pin modules to versions in our examples because of the
difficulty of keeping the versions in the documentation in sync with the latest released versions.
We highly recommend that in your code you pin the version to the exact version you are
using so that your infrastructure remains stable, and update versions in a
systematic way so that they do not catch you by surprise.

Also, because of a bug in the Terraform registry ([hashicorp/terraform#21417](https://github.com/hashicorp/terraform/issues/21417)),
the registry shows many of our inputs as required when in fact they are optional.
The table below correctly indicates which inputs are required.


Amazon Simple Notification Service (Amazon SNS) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients.
[SNS documentation](https://docs.aws.amazon.com/sns/latest/dg/sns-how-it-works.html)

There are many ways SNS can be used. As an example we can imagine CloudWatch sending alerts to SNS, by using subscribers such notifications
can be sent further to PagerDuty, OpsGenie or any other oncall management tool.



## Examples

```hcl
module "sns" {
  source = "git::https://github.com/cloudposse/terraform-aws-sns-topic.git?ref=0.0.1"

  attributes = var.attributes
  name       = var.name
  namespace  = var.namespace
  stage      = var.stage

  subscribers = {
    opsgenie = {
      protocol = "https"
      endpoint = "https://api.example.com/v1/"
      endpoint_auto_confirms = true
    }
  }

  sqs_dlq_enabled = false
}
```



<!-- markdownlint-disable -->

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.13.0 |
| aws | >= 2.0 |

## Providers

| Name | Version |
|------|---------|
| aws | >= 2.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| this | cloudposse/label/null | 0.24.1 |

## Resources

| Name |
|------|
| [aws_caller_identity](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) |
| [aws_iam_policy_document](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) |
| [aws_sns_topic](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic) |
| [aws_sns_topic_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_policy) |
| [aws_sns_topic_subscription](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_subscription) |
| [aws_sqs_queue](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sqs_queue) |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| additional\_tag\_map | Additional tags for appending to tags\_as\_list\_of\_maps. Not added to `tags`. | `map(string)` | `{}` | no |
| allowed\_aws\_services\_for\_sns\_published | AWS services that will have permission to publish to SNS topic. Used when no external json policy is used. | `list(string)` | <pre>[<br>  "cloudwatch.amazonaws.com"<br>]</pre> | no |
| allowed\_iam\_arns\_for\_sns\_publish | IAM role/user ARNs that will have permission to publish to SNS topic. Used when no external json policy is used. | `list(string)` | `[]` | no |
| attributes | Additional attributes (e.g. `1`) | `list(string)` | `[]` | no |
| context | Single object for setting entire context at once.<br>See description of individual variables for details.<br>Leave string and numeric variables as `null` to use default value.<br>Individual variable settings (non-null) override settings in context object,<br>except for attributes, tags, and additional\_tag\_map, which are merged. | `any` | <pre>{<br>  "additional_tag_map": {},<br>  "attributes": [],<br>  "delimiter": null,<br>  "enabled": true,<br>  "environment": null,<br>  "id_length_limit": null,<br>  "label_key_case": null,<br>  "label_order": [],<br>  "label_value_case": null,<br>  "name": null,<br>  "namespace": null,<br>  "regex_replace_chars": null,<br>  "stage": null,<br>  "tags": {}<br>}</pre> | no |
| delimiter | Delimiter to be used between `namespace`, `environment`, `stage`, `name` and `attributes`.<br>Defaults to `-` (hyphen). Set to `""` to use no delimiter at all. | `string` | `null` | no |
| delivery\_policy | The SNS delivery policy as JSON. | `string` | `null` | no |
| enabled | Set to false to prevent the module from creating any resources | `bool` | `null` | no |
| environment | Environment, e.g. 'uw2', 'us-west-2', OR 'prod', 'staging', 'dev', 'UAT' | `string` | `null` | no |
| id\_length\_limit | Limit `id` to this many characters (minimum 6).<br>Set to `0` for unlimited length.<br>Set to `null` for default, which is `0`.<br>Does not affect `id_full`. | `number` | `null` | no |
| kms\_master\_key\_id | The ID of an AWS-managed customer master key (CMK) for Amazon SNS or a custom CMK | `string` | `"alias/aws/sns"` | no |
| label\_key\_case | The letter case of label keys (`tag` names) (i.e. `name`, `namespace`, `environment`, `stage`, `attributes`) to use in `tags`.<br>Possible values: `lower`, `title`, `upper`.<br>Default value: `title`. | `string` | `null` | no |
| label\_order | The naming order of the id output and Name tag.<br>Defaults to ["namespace", "environment", "stage", "name", "attributes"].<br>You can omit any of the 5 elements, but at least one must be present. | `list(string)` | `null` | no |
| label\_value\_case | The letter case of output label values (also used in `tags` and `id`).<br>Possible values: `lower`, `title`, `upper` and `none` (no transformation).<br>Default value: `lower`. | `string` | `null` | no |
| name | Solution name, e.g. 'app' or 'jenkins' | `string` | `null` | no |
| namespace | Namespace, which could be your organization name or abbreviation, e.g. 'eg' or 'cp' | `string` | `null` | no |
| regex\_replace\_chars | Regex to replace chars with empty string in `namespace`, `environment`, `stage` and `name`.<br>If not set, `"/[^a-zA-Z0-9-]/"` is used to remove all characters other than hyphens, letters and digits. | `string` | `null` | no |
| sns\_topic\_policy\_json | The fully-formed AWS policy as JSON | `string` | `""` | no |
| sqs\_dlq\_enabled | Enable delivery of failed notifications to SQS and monitor messages in queue. | `bool` | `false` | no |
| sqs\_dlq\_max\_message\_size | The limit of how many bytes a message can contain before Amazon SQS rejects it. An integer from 1024 bytes (1 KiB) up to 262144 bytes (256 KiB). The default for this attribute is 262144 (256 KiB). | `number` | `262144` | no |
| sqs\_dlq\_message\_retention\_seconds | The number of seconds Amazon SQS retains a message. Integer representing seconds, from 60 (1 minute) to 1209600 (14 days). | `number` | `1209600` | no |
| sqs\_queue\_kms\_data\_key\_reuse\_period\_seconds | The length of time, in seconds, for which Amazon SQS can reuse a data key to encrypt or decrypt messages before calling AWS KMS again | `number` | `300` | no |
| sqs\_queue\_kms\_master\_key\_id | The ID of an AWS-managed customer master key (CMK) for Amazon SQS Queue or a custom CMK | `string` | `"alias/aws/sqs"` | no |
| stage | Stage, e.g. 'prod', 'staging', 'dev', OR 'source', 'build', 'test', 'deploy', 'release' | `string` | `null` | no |
| subscribers | Required configuration for subscibres to SNS topic. | <pre>map(object({<br>    protocol = string<br>    # The protocol to use. The possible values for this are: sqs, sms, lambda, application. (http or https are partially supported, see below) (email is an option but is unsupported, see below).<br>    endpoint = string<br>    # The endpoint to send data to, the contents will vary with the protocol. (see below for more information)<br>    endpoint_auto_confirms = bool<br>    # Boolean indicating whether the end point is capable of auto confirming subscription e.g., PagerDuty (default is false)<br>    raw_message_delivery = bool<br>    # Boolean indicating whether or not to enable raw message delivery (the original message is directly passed, not wrapped in JSON with the original message in the message property) (default is false)<br>  }))</pre> | `{}` | no |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')` | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| aws\_sns\_topic\_subscriptions | SNS topic subscriptions |
| sns\_topic | SNS topic |
<!-- markdownlint-restore -->




