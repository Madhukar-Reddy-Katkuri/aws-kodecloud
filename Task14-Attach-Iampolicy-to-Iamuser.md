# Attach IAM policy to IAM user

An IAM user named iamuser_john and a policy named iampolicy_john already exist. Attach the IAM policy iampolicy_john to the IAM user iamuser_john.

### Solution

```
# Get the ARN of the iampolicy_john policy
POLICY_ARN=$(aws iam list-policies --query "Policies[PolicyName=='iampolicy_john'].Arn" --output text)

# Attach the policy to the iamuser_john user
aws iam attach-user-policy --policy-arn $POLICY_ARN --user-name iamuser_john

```

## Verification

```
aws iam list-attached-user-policies --user-name iamuser_john

```

## To get policy document

```
# Get the ARN of the iampolicy_john policy
POLICY_ARN=$(aws iam list-policies --query "Policies[?PolicyName=='iampolicy_john'].Arn" --output text)

# Describe the policy metadata
aws iam get-policy --policy-arn $POLICY_ARN

# Get the default version ID of the policy
VERSION_ID=$(aws iam get-policy --policy-arn $POLICY_ARN --query "Policy.DefaultVersionId" --output text)

# Describe the policy document (JSON format)
aws iam get-policy-version --policy-arn $POLICY_ARN --version-id $VERSION_ID --query "PolicyVersion.Document" --output json

```

**Metadata**: The output of ```aws iam get-policy``` will provide general information about the policy.

**Policy Document**: The output of ``aws iam get-policy-version`` will show the actual permissions defined in the policy document in JSON format.