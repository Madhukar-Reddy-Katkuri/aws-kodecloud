# Create a Subnet

**Requirements**

- Create a subnet named **nautilus-subnet** under default VPC

## Solution

```
aws ec2 create-subnet --vpc-id vpc-03b9df441e1f92770 --cidr-block 172.31.111.0/28 --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=nautilus-subnet}]'
```

```
aws ec2 describe-subnets --subnet-ids subnet-005dc661a8d910bcb
```
