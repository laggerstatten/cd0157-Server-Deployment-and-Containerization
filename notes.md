### configure AWS

aws configure
aws configure set region us-east-2




setx AWS_ACCESS_KEY_ID AKIAZA2J2JNLAV3573HD
setx AWS_SECRET_ACCESS_KEY 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
setx AWS_DEFAULT_REGION us-east-2








### run app locally

python main.py
curl --request GET http://localhost:8080/


### build docker image / create and run container

docker rm $(docker ps -a -q)

docker build -t myimage .

docker run --name myContainer --env-file=.env_file -p 80:8080 myimage

curl --request GET 'http://localhost:80/'

### create EKS cluster

kubectl version  
eksctl create cluster --name simple-jwt-api --nodes=2 --version=1.23 --instance-types=t2.medium --region=us-east-2

kubectl get nodes

#### to delete
eksctl delete cluster simple-jwt-api  --region=us-east-2


### create IAM Role for CodeBuild

aws sts get-caller-identity --query Account --output text

aws iam create-role --role-name UdacityFlaskDeployCBKubectlRole --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'
aws iam put-role-policy --role-name UdacityFlaskDeployCBKubectlRole --policy-name eks-describe --policy-document file://iam-role-policy.json

### authorize CodeBuild

kubectl get -n kube-system configmap/aws-auth -o yaml > aws-auth-patch.yml

kubectl patch configmap/aws-auth -n kube-system --patch "$(cat aws-auth-patch.yml)"

kubectl get nodes





aws ssm put-parameter --name JWT_SECRET --overwrite --value "myjwtsecret" --type SecureString
## Verify
aws ssm get-parameter --name JWT_SECRET
















