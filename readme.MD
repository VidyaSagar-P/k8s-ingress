# Ingress controller

### OIDC Provider
- OIDC = OpenID Connect (used for authentication between AWS IAM and Kubernetes).
REGION_CODE=us-east-1 CLUSTER_NAME=roboshop ACC_ID=697576974187
```
eksctl utils associate-iam-oidc-provider \
    --region $REGION_CODE \
    --cluster $CLUSTER_NAME \
    --approve
```
EKS and IAM can now “trust” each other for authentication.

### Download an IAM policy
- You’re downloading a JSON policy file that defines what permissions the AWS Load Balancer Controller needs.
```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.14.1/docs/install/iam_policy.json
```

### Create IAM Policy
- You’re creating a new IAM policy in your AWS account using the file you just downloaded.
```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```
This policy gives the controller permission to:Create and modify ALBs, Register targets (pods), Manage security groups, etc.

### Create an IAM role and Kubernetes SA
```
eksctl create iamserviceaccount \
--cluster=$CLUSTER_NAME \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::$ACC_ID:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region $REGION_CODE \
--approve
```
Kubernetes → IAM Role connection established.
Controller Pods will use this Service Account to authenticate with AWS.


```
sudo dnf install helm -y
```

### Download drivers
```
helm repo add eks https://aws.github.io/eks-charts
```

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=$CLUSTER_NAME --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```