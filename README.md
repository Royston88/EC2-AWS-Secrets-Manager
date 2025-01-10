# EC2-AWS-Secrets-Manager
A writeup on how to authorize an EC2 instance to retrieve secrets from AWS Secrets Manager.

To authorize an **EC2 instance** to retrieve secrets from **AWS Secrets Manager**, you'll need to set up the following:

1. **IAM Role**: The EC2 instance should have an IAM role attached to it with the necessary permissions to access AWS Secrets Manager. This is done by granting the EC2 instance role permissions through an IAM policy.

2. **IAM Policy**: The IAM policy grants the required permissions for the EC2 instance to access the specific secret from Secrets Manager.

3. **Secret ARN**: You'll specify the ARN (Amazon Resource Name) of the secret in the IAM policy to restrict access to the desired secret.

# IAM Policy for Access to AWS Secrets Manager
To allow your EC2 instance to retrieve a specific secret, you can create an IAM policy with the ```secretsmanager:GetSecretValue``` permission for the secret's ARN.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:REGION:ACCOUNT_ID:secret:prod/cart-service/credentials-*"
        }
    ]
}
```

# Deriving the ARN for the Secret
To derive the ARN for the secret named ```prod/cart-service/credentials```, you need to follow the structure of an AWS Secrets Manager ARN:
```ruby
arn:aws:secretsmanager:<region>:<account-id>:secret:<secret-name>-<random-suffix>
```
Assuming the secret is in the ```us-east-1``` region and your AWS account ID is ```123456789012```, the ARN for the secret would look like this:
```ruby
arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials-ABC123
```

# Final IAM Policy with Specific ARN
Based on the secret name ```prod/cart-service/credentials```, your IAM policy for the EC2 role would look like this, assuming the secret is in the ```us-east-1``` region and account ID ```123456789012```:

```json
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
```

# Attach the IAM Role to the EC2 Instance
Once you've created the IAM policy, you can:

1. Create an IAM role with the policy above.
2. Attach the IAM role to your EC2 instance.

This will authorize the EC2 instance to retrieve the secret from Secrets Manager.
