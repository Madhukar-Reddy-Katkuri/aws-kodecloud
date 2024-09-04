# Create a IAMrole and attach IAM policy

Create an IAM role as below:

1) IAM role name must be iamrole_javed.
2) Entity type must be AWS Service and use case must be EC2.
3) Attach a policy named iampolicy_javed.

## Create the Trust Policy for EC2
The trust relationship defines which AWS service (in this case, EC2) can assume the role. You need a trust policy that allows EC2 instances to assume this role.

Save the following trust policy to a JSON file (e.g., trust-policy.json):

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
## Create the IAM Role
Use the following AWS CLI command to create the IAM role iamrole_javed and associate the trust policy

```
aws iam create-role --role-name iamrole_javed --assume-role-policy-document file://trust-policy.json
```
## Attach the Policy iampolicy_javed to the Role

Before attaching the policy, ensure you have the ARN of the iampolicy_javed. You can retrieve it with the following command:
```
POLICY_ARN=$(aws iam list-policies --query "Policies[?PolicyName=='iampolicy_javed'].Arn" --output text)
```
## Once you have the ARN, attach the policy to the role using this command
```
aws iam attach-role-policy --role-name iamrole_javed --policy-arn $POLICY_ARN
```

# Full Command Sequence

```
# Step 1: Create the trust policy for EC2 and save to trust-policy.json
cat > trust-policy.json <<EOL
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOL

# Step 2: Create the IAM role iamrole_javed
aws iam create-role --role-name iamrole_javed --assume-role-policy-document file://trust-policy.json

# Step 3: Get the ARN of iampolicy_javed
POLICY_ARN=$(aws iam list-policies --query "Policies[?PolicyName=='iampolicy_javed'].Arn" --output text)

# Step 4: Attach the iampolicy_javed policy to iamrole_javed
aws iam attach-role-policy --role-name iamrole_javed --policy-arn $POLICY_ARN
```
# Verification
To verify that the role and policy attachment were successful, you can list the policies attached to the role:
```
aws iam list-attached-role-policies --role-name iamrole_javed
```

```
~ on ☁️  (us-east-1) ➜  aws iam list-attached-role-policies --role-name iamrole_javed
{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_javed",
            "PolicyArn": "arn:aws:iam::767397778924:policy/iampolicy_javed"
        }
    ]
}

~ on ☁️  (us-east-1) ➜  
```