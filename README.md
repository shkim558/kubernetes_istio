k8s 시스템 구축(AWS EKS-EC2)
1. EC2 키 생성
    ```
    aws ec2 create-key-pair --region us-west-2 --key-name ec2-key --output text > ec2-key.pem
    ```
2. EKS 클러스터 생성 (AWS Cloudformation)
    ```
    eksctl create cluster -f eks-cluster.yaml
    ```
3. istio profile(demo) install
    ```
    istioctl manifest generate --set profile=demo > istio-profile.yaml
    ```
4. istio profile apply
    ```
    kubectl apply -f istio-profile.yaml
    ```
5. istio namespace(default) label auto injection
   ```
   kubectl label namespace default istio-injection=enabled
   ```
6. AWS load balancer controller install
   ```
    helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
    -n kube-system \
    --set clusterName=<my-eks-cluster> \
    --set serviceAccount.create=false \
    --set serviceAccount.name=aws-load-balancer-controller \
    --set image.repository=602401143452.dkr.ecr.us-west-2.amazonaws.com/amazon/aws-load-balancer-controller
    ```
7. istio gateway, virtualService 적용
    ```
    kubectl apply -f istio/gateway/sentry.yaml
    kubectl apply -f istio/virtualService/sentry.yaml
    ```