Q1) What is needed to authorize your EC2 to retrieve secrets from the AWS Secret Manager?

A1) To authorize our EC2 instance to retrieve secrets from AWS Secrets Manager, we need to perform the following steps:

   1. Create an IAM Role for the EC2 Instance
    We need an IAM role that will be attached to the EC2 instance. This role will grant permissions for the 
    EC2 instance to interact with AWS Secrets Manager.

   2. Attach the IAM Policy to the IAM Role
     We need to create and attach an IAM policy that grants permission to access the specific secret(s) stored in 
     AWS Secrets Manager.

   3. Attach the IAM Role to the EC2 Instance
     After creating the IAM role with the necessary policy, we need to attach this IAM role to our EC2 instance:

    When launching a new EC2 instance, we can specify the IAM role during the instance creation process.
    If the instance is already running, we can attach the IAM role using the EC2 Management Console or the AWS CLI.

    4. Configure the EC2 Instance to Retrieve the Secret
    Once the IAM role is attached, our EC2 instance can retrieve the secret using AWS SDKs or the AWS CLI.


Q2. Derive the IAM policy (i.e. JSON)?
   - Using the secret name prod/cart-service/credentials, derive a sensible ARN as the specific resource for access

A2) To derive the IAM policy for accessing a secret in AWS Secrets Manager, we need to create a policy that 
    grants the required permissions (e.g., secretsmanager:GetSecretValue) to the specific secret. 
    The ARN (Amazon Resource Name) for the secret depends on the secret name and our AWS account details.

   Given the secret name prod/cart-service/credentials, here’s how to create an ARN and IAM policy.

   1. Constructing the ARN for the Secret
  The ARN format for a secret in AWS Secrets Manager is:

  For example, assuming:

  Region: us-east-1
  Account ID: 123456789012
  The generated suffix might be something like AbCdEf 
  Thus, the ARN might look like:

   arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials-AbCdEf

   2. IAM Policy for Accessing the Secret
    Now, we can create an IAM policy that allows the EC2 instance to retrieve the secret using the 
    secretsmanager:GetSecretValue action.

   Here's the IAM policy in JSON format:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials-*"
        }
    ]
}

Explanation:
Action: secretsmanager:GetSecretValue grants permission to retrieve the value of the secret.
Resource: The ARN of the secret is specified, with prod/cart-service/credentials-* allowing access to the secret, 
including the random suffix.
Effect: Allow grants permission.
