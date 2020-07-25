# AWS CloudFormation
cloudformation template to create Resources in AWS  

## Create Stack from CLI with CloudFormation template
```
aws cloudformation create-stack --profile bsejawal --stack-name vpc --template-body vpc.yml 
aws cloudformation create-stack --profile bsejawal --stack-name iam --template-body iam.yml  --capabilities CAPABILITY_IAM
aws cloudformation create-stack --profile bsejawal --stack-name appCluster --template-file app-cluster.yml 
```

## Delete Stack From CLI
```
aws cloudformation delete-stack --profile bsejawal --stack-name app-cluster
```


## Docker Related Commands

### Make image
```
docker build -t bhesh-demo-ecr .
```


### Run the image
```
winpty docker run -it -p 8070:8080 --rm bhesh-demo-ecr:latest
```

### Create ECR repository
```
aws ecr create-repository --repository-name bhesh-demo-ecr --profile bsejawal
```

### Get the ECR ARN
```
aws ecr describe-repositories --repository-name bhesh-demo-ecr --profile bsejawal
```

### Docker login in aws ECR
```
aws ecr get-login-password --profile bsejawal | docker login --username AWS --password-stdin 797013890234.dkr.ecr.us-east-1.amazonaws.com
```

### Tag the image
```
docker tag bhesh-demo-ecr:latest 797013890234.dkr.ecr.us-east-1.amazonaws.com/bhesh-demo-ecr:v1
```

### Push to ECR Repository
```
docker push 797013890234.dkr.ecr.us-east-1.amazonaws.com/bhesh-demo-ecr:v1
```

