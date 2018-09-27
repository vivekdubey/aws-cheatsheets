## 1. List Security groups attached to  an ALB:
```
aws --profile $PROFILE --region $REGION elbv2 describe-load-balancers --load-balancer-arns $ALB_ARN | jq '.[][] | .SecurityGroups | .[]' -r
```
## 2. List IPs in all security groups attached to an ALB:
```
aws --profile $PROFILE --region $REGION elbv2 describe-load-balancers --load-balancer-arns $ALB_ARN | jq '.[][] | .SecurityGroups | .[]' -r | xargs -I{}  aws --profile $PROFILE --region $REGION ec2 describe-security-groups --group-ids {} | jq '.[][] | .IpPermissions | .[] | .IpRanges | .[] | .CidrIp' -r
``` 

