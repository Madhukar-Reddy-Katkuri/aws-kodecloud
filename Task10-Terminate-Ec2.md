# Terminate Ec2 Instance

## Solution

1. Get Instance ID

```
INSTANCE_ID=$(aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[*].Instances[*].InstanceId" --output text)

```

2. Terminate the Instance

```
aws ec2 terminate-instances --region us-east-1 --instance-ids $INSTANCE_ID

```

3. Wait for the Instance to Terminate

```
while state=$(aws ec2 describe-instances --region us-east-1 --instance-ids $INSTANCE_ID --query "Reservations[*].Instances[*].State.Name" --output text); [ "$state" != "terminated" ]; do
  echo "Waiting for EC2 to terminate: $state"
  sleep 10
done

echo "EC2 terminated"

```

## Verification

```
aws ec2 describe-instances --region us-east-1 --instance-ids $INSTANCE_ID --query "Reservations[*].Instances[*].{InstanceId:InstanceId,State:State.Name}" --output table

```

**Explanation**

- **{InstanceId:InstanceId}**: Extracts the InstanceId.

- **{State:State.Name}**: Extracts the current state of the instance.
