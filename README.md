# lambda-tf

**Info**
------
This Terraform module creates a Lambda function with a minimum set of resource required to cover default functionalities.

**_Note:_** This module expects s3_key value to be provided in order to deploy code to the Lambda function.

**Usage**
------
```python
module "my-lambda" {
  source = "github.com/sebolabs/lambda-tf.git"

  project     = "lab"
  environment = "test"
  component   = "my-app"
  name        = "my-lambda"

  s3_bucket = "my-bucket"
  s3_key    = "packages/my-package.zip"

  runtime     = "nodejs6.10"
  handler     = "handler.fire"
  memory_size = "128"
  timeout     = "15"
  publish     = false

  principal_service = "apigateway"

  invoker_source_arn = "${format(
    "%s:%s:%s:%s/%s",
    "arn:aws:execute-api",
    var.aws_region,
    data.aws_caller_identity.current.account_id,
    aws_api_gateway_rest_api.main.id,
    "*/*/*"
  )}"

  env_variables = "${merge(
    var.env_vars,
    map(
      "MY_ENV", "TEST"
    )
  )}"

  cwlg_retention_in_days = 3
}
```

**Terraform compatibility**
------
TF versions tested: 0.10.7
