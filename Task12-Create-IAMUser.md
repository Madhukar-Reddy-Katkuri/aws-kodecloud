# Create IAM User

create an IAM user named iamuser_mark

## Solution

```
aws iam create-user --user-name iamuser_mark
```

**To get user info**

```
aws iam get-user --user-name iamuser_mark
```