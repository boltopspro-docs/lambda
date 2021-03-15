<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/lambda/blob/master/README.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# Lambda Function CloudFormation Blueprint

[![Watch the video](https://img.boltops.com/boltopspro/video-preview/blueprints/lambda-2.png)](https://www.youtube.com/watch?v=yrIh9YzlDrM)

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

![CodeBuild](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiVDU4ckFJMTNNZDdhZnYxOUErMncvSW5GK2VpZ24xRVB1SjVteXhrYTA4cHU2U3N0VmtOS0lxYXQrZzFLeW5MUkNFaXlSUkVlRGxuU3lNWFlBRjYzYlRBPSIsIml2UGFyYW1ldGVyU3BlYyI6ImVnUE0rZ0tNOTZGOHF3dU0iLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

This blueprint provisions a [Lambda Function](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html) and [CloudWatch Event Rule](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-rule.html). The example does the heavy lifting by providing some good code structure.  It can be uses an an example to to be modified and fit your needs.

* Includes an example of Lambda Layer
* Includes necessary Lambda Permission and IAM Role
* Optionally supports [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html#cfn-lambda-function-vpcconfig). Variables can be used to set different values for multiple environments.
* Supports [Lambda X-Ray tracing](https://docs.aws.amazon.com/lambda/latest/dg/lambda-x-ray.html)
* Can customize [ScheduledExpression](https://docs.aws.amazon.com/eventbridge/latest/userguide/scheduled-events.html) or [EventPattern](https://docs.aws.amazon.com/eventbridge/latest/userguide/aws-events.html).

## Usage

1. Add blueprint to Gemfile
2. Configure: configs/lambda values
3. Deploy blueprint

## Add

Add the blueprint to your lono project's `Gemfile`.

```ruby
gem "lambda", git: "git@github.com:boltopspro/lambda.git"
```

## Configure

Use the [lono seed](https://lono.cloud/reference/lono-seed/) command to generate a starter config params files.

    LONO_ENV=development lono seed lambda
    LONO_ENV=production  lono seed lambda

The files in `config/lambda` folder will look something like this:

    configs/lambda/
    └── variables
        ├── development.rb
        └── production.rb

Configure the `configs/lambda/variables` files.

## Deploy

Use the [lono cfn deploy](http://lono.cloud/reference/lono-cfn-deploy/) command to deploy.

    LONO_ENV=development lono cfn deploy lambda --sure --no-wait
    LONO_ENV=production  lono cfn deploy lambda --sure --no-wait

If you are using One AWS Account, use these commands instead: [One Account](docs/one-account.md).

## Configure Details

### Lambda Runtime

The default Lambda Runtime for this blueprint is `ruby2.7`. You can adjust this with the `@lambda_runtime` variable.

```ruby
@lambda_runtime = "ruby2.5"
```

IMPORTANT: Make sure that the version of ruby you are using matches. Lono uses your ruby version to build the autmoatically build the Lambda Layer. To check your ruby version:

    $ ruby -v
    ruby 2.5.7p206 (2019-10-01 revision 67816) [x86_64-linux]
    $

Make sure to run [lono clean](https://lono.cloud/reference/lono-clean/) also if you are changing ruby versions as the lambda layer could be cached in output.

    lono clean

If you change the `@lambda_runtime` to other languages, you'll need to change the code also: [app/files/lambda-function](app/files/lambda-function). You will also need to handle building and adding the files for the Lambda Layer.

### Ruby Specific Tips

With ruby2.5 when the `json` is in your Gemfile it confuses ruby and it is unable to require json. Believe this is a bug in ruby2.5 itself.  Either remove json from your Gemfile or use the ruby2.7 runtime.

### Configure Details

Refer to the [lono lambda extension](https://github.com/boltopspro-docs/lambda_extension) for details on how to configure the Schedule Expression, VPC, X-Ray Tracing, etc.
