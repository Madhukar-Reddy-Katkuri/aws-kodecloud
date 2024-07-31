# Create a Snapshot of the Volume

Create a snapshot of an existing volume named nautilus-vol in us-east-1 region.

1) The name of the snapshot must be nautilus-vol-ss.

2) The description must be nautilus Snapshot.

3) Make sure the snapshot status is completed before submitting the task.

## Solution

**Describe and get the volume snapshotID**

```
INSTANCE_ID=$(aws ec2 describe-volumes --region us-east-1 --filters "Name=tag:Name,Values=nautilus-vol" --query "Volumes[*].VolumeId" --output text)
```

**Create snapshot**

```
aws ec2 create-snapshot --volume-id $INSTANCE_ID --region us-east-1 --description "nautilus snapshot" --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Values=nautilus-vol-ss}]'
```

**Check the snapshot status**

```
while state=$(aws ec2 describe-snapshots --region us-east-1 --snapshot-ids snap-067ed31be593d9dec --query "Snapshots[*].State" --output text); [ "$state" != "completed" ]; do
  echo "Waiting for snapshot to complete: $state"
  sleep 10
done
echo "Snapshot is $state"
```

**Verification**

```
aws ec2 describe-snapshots --region us-east-1 --snapshot-ids snap-067ed31be593d9dec --query "Snapshots[*].{SnapshotId:SnapshotId,State:State,VolumeId:VolumeId}" --output table
```
