# Create an Read only IAM policy for Ec2 Instance

Create an IAM policy named iampolicy_javed in us-east-1 region, it must allow read-only access to the EC2 console, i.e this policy must allow users to view all instances, AMIs, and snapshots in the Amazon EC2 console.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:GetConsoleOutput",
                "ec2:GetConsoleScreenshot"
            ],
            "Resource": "*"
        }
    ]
}
```

1. Create a Policy JSON
2. Create the IAM policy using AWS CLI
Save the above JSON to a file named ec2-read-only-policy.json.

```
cat > ec2-read-only-policy.json <<EOL
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:GetConsoleOutput",
                "ec2:GetConsoleScreenshot"
            ],
            "Resource": "*"
        }
    ]
}
EOL

```

```
aws iam create-policy --policy-name iampolicy_javed --policy-document file://ec2-read-only-policy.json --region us-east-1

```

## Verification

```
aws iam list-policies --query "Policies[PolicyName=='iampolicy_javed'"

```