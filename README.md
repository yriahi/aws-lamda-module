# AWS Lambda module


## Example

```bash
module "example_lambda_instance" {
  source     = "github.com/yriahi/aws-lamda-module//lambda?ref=1.0.0"
  # packaged zip file
  package    = "${path.module}/dist/foo.zip"
  name       = "${var.name_prefix}-foo-name"
  human_name = "Latest Foo Name Here (${var.name_prefix})"
  handler    = "handler.handler"
  runtime    = "python3.8"
  timeout    = 300
  # additional policies
  iam_policies = [
    data.aws_iam_policy_document.parameter_store_policy.json,
  ]
  environment = {
    variables = {
      SSM_PARAMETER_NAME = var.golden_ami_ssm_parameter_path
      ENVIRONMENT_NAME   = var.environment_name
    }
  }
  tags = var.tags
}



data "aws_iam_policy_document" "parameter_store_policy" {
  statement {
    effect = "Allow"
    actions = [
      "ssm:GetParameters",
      "ssm:PutParameter",
      "ssm:AddTagsToResource"
    ]
    resources = ["arn:aws:ssm:*:${data.aws_caller_identity.current.account_id}:parameter${var.bar}"]
  }
}
```
