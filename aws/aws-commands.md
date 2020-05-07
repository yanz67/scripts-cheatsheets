## AWS Commands

### Create binary secret in Secrets Manager
```
aws secretsmanager create-secret --profile $AWS_PROFILE  \
  --name secret_name --secret-binary fileb://mykeystore.jks
```


### Update binary secret in Secrets Manager
```
aws secretsmanager put-secret-value --profile $AWS_PROFILE  \
  --secret-id secret_name --secret-binary fileb://mykeystore.jks
```

### Get size of all EBS volumes in the account
```
aws ec2 describe-volumes \
  --profile $AWS_PROFILE \
  --query 'sum(Volumes[].Size)'
```
### Get EC2 instances DNS names based on a Tag filter
```
aws ec2 describe-instances \
  --profile $AWS_PROFILE \
  --region us-east-2 \
  --filter Name=tag:Name,Values="Test*" \
  --query 'Reservations[*].Instances[*].PublicDnsName'
```
### Get CfnOutput set in AWS cdk from cloudformation stack
```
aws cloudformation describe-stacks \
  --profile "$AWS_PROFILE" \
  --stack-name "$STACK_NAME \
  --query "Stacks[0].Outputs[?OutputKey=='$KEY_NAME'].OutputValue" \
  --output text
``` 
### GET running instances information
```
aws ec2 describe-instances \
  --profile "$AWS_PROFILE" \
  --region us-east-1 \
  --filters Name=instance-state-name,Values=running \
  --query 'Reservations[*].Instances[*].[Tags[*].Value, InstanceId, PublicDnsName]'
```
### GET Instances and ouput specific tag's value
```
aws ec2 describe-instances
--profile "$AWS_PROFILE" 
--region us-east-2  
--filter Name=tag:Name,Values="*" 
--query 'Reservations[*].Instances[*].[Tags[?Key==`Name`].Value]'
```

### Create private key for EC2 instances  
```
aws ec2 --profile "$AWS_PROFILE" create-key-pair --key-name KEY_NAME
```

### Output Instance Name Tags and Instance Status filter by specific tag values
```
aws --profile $AWS_PROFILE ec2 describe-instances \
  --filter Name=tag:Name,Values="$DEPLOY_ENVIRONMENT-*" \
  --query 'Reservations[].Instances[].{"Instance Name": Tags[?Key==`Name`] | [0].Value, State: State.Name}' \
  --output table
```