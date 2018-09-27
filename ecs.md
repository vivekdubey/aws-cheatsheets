## 1. List services in an ECS cluster
```
aws --profile $PROFILE --region $REGION ecs list-services --cluster $CLUSTER_NAME --output text | awk '{print $2}'
```

## 2. Get latest container image version deployed to an ECS service 
```
aws --profile $PROFILE --region $REGION ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_ARN | jq '.[][] | .taskDefinition' -r | xargs -I{} aws ecs --profile $PROFILE --region $REGION describe-task-definition --task-definition {} | jq '.[] | .containerDefinitions | .[] | .image' -r
```

## 3. Get lastest docker container version deployed for all services in a ECS cluster
```
aws --profile $PROFILE --region $REGION ecs list-services --cluster $CLUSTER_NAME --output text | awk '{print $2}' | xargs -I{} aws --profile $PROFILE --region $REGION ecs describe-services --cluster $CLUSTER_NAME --services {} | jq '.[][] | .taskDefinition' -r | xargs -I{} aws ecs --profile $PROFILE --region $REGION describe-task-definition --task-definition {} | jq '.[] | .containerDefinitions | .[] | .image' -r
```
