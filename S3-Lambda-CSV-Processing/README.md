# TODO
```
[X] Secrets Manager integration with Lambda (PG and Private Key)
[X] Schedule Lambda to Run
[X] CloudFormation template

[] Update README with more screenshots

[] Review Permissions for Lambda
   * Note that the Secrets Manager permission is overly permissive (no need for write)
[] Add SNS notification each time the Lambda finishes

[] Error handling on Lambda
[] Convert the JS to a Lambda
[] Deploy lambda code as zip file from S3 bucket
```

# Overview
The following CloudFormation template can be used to process batch data which is uploaded
into an S3 bucket and then processed by a Lambda function.
The appropriate roles and permissions are created in the template and no modification should be required.

## Setup
Install and configure the AWS CLI for ease of use.

Run the following command from this directory to deploy the cloudformation template.

```
aws cloudformation deploy --stack-name CSVProcessing --template-file cloudformation.yml --capabilities CAPABILITY_NAMED_IAM
```

## Testing

### Secrets Manager
Create a secret in the `us-east-2` region called `test-secret`.
This can be a Postgres secret for example.

The Lambda will log the results to the console. 


### S3 bucket and basic functionality
The Lambda function can be tested by uploading the test-data.csv file to the bucket.

Note: you might need to update the bucket name, ARN, and object name depending on your setup.
A test event can be created on the Lambda function such as 

```
{
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "aws:s3",
      "awsRegion": "us-east-1",
      "eventTime": "1970-01-01T00:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "userIdentity": {
        "principalId": "EXAMPLE"
      },
      "requestParameters": {
        "sourceIPAddress": "127.0.0.1"
      },
      "responseElements": {
        "x-amz-request-id": "EXAMPLE123456789",
        "x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "testConfigRule",
        "bucket": {
          "name": "CSVs-data-9874534",
          "ownerIdentity": {
            "principalId": "EXAMPLE"
          },
          "arn": "arn:aws:s3:::CSVs-data-9874534"
        },
        "object": {
          "key": "test-data.csv",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}
```

You should upload a file into your newly created S3 bucket called `test-data.csv` in this example to run the test from the Lambda UI.

## Delete Stack
```
aws cloudformation delete-stack --stack-name CSVProcessing
```
