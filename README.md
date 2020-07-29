# AWS CloudFormation
cloudformation template to create Resources in AWS  

### Remove image if already exists locallly 
```
docker image rm $(docker images | grep bhesh-demo-ecr | tr -s ' ' | cut -d " " -f 3)
```

### Delete all tags and an image in ECR
```
 aws ecr batch-delete-image --profile bsejawal --repository-name bhesh-demo-ecr --image-ids imageDigest=$(aws ecr list-images --profile bsejawal --repository-name bhesh-demo-ecr | grep imageDigest | cut -d '"' -f4)
 ```
 
### Now delete Stack
```
aws ecr delete-repository --profile bsejawal --repository-name bhesh-demo-ecr --force
OR Delete Stack
aws cloudformation delete-stack --profile bsejawal --stack-name app-cluster
``` 

### Get the code and build artifact
```
git clone git@github.platforms.engineering:MBT/bhesh-demo.git
cd crud
mvn clean package
cd targate
```

### Build image
```
docker build -t bhesh-demo-ecr .
```


### Run the image
```
winpty docker run -it -p 8070:8080 --rm bhesh-demo-ecr:latest
CTRL+C to stop
```
### Create Stack from CLI with CloudFormation template
```
aws cloudformation create-stack --profile bsejawal --template-body file://vpc.yml --stack-name vpc
aws cloudformation create-stack --profile bsejawal --stack-name iam --template-body file://iam.yml  --capabilities CAPABILITY_IAM
aws cloudformation create-stack --profile bsejawal --stack-name appCluster --template-body file://app-cluster.yml
aws cloudformation create-stack --profile bsejawal --stack-name codebuild --template-body file://codebuild.yml 
aws cloudformation create-stack --profile bsejawal --stack-name api --template-body file://api.yml 
```
### Get Webhook URL and Tocken
```
aws codebuild create-webhook --profile bsejawal --project-name bhesh-demo --branch-filter master 
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
