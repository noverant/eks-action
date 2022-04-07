eks-action
=============
Interacts with kubernetes clusters on eks by `kubectl` commands.

## Usage

### EKS Example
```yml
name: Deploy

on:
  push:
    branches:
      - develop

env:
  DOCKER_IMAGE: docker_image
  DOCKER_TAG: latest
  K8S_NAMESPACE: dev_namespace
  K8S_DEPLOYMENT: dev_deployment
  AWS_REGION: ap-northeast-2
  EKS_CLUSTER_NAME: dev_cluster
  EKS_ROLE_ARN: arn:aws:iam::XXXXXXXXXXXX:role/testrole

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Deploy
        uses: cancue/eks-action@v0.0.2
        env:
          aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region: $AWS_REGION
          cluster_name: $EKS_CLUSTER_NAME
          role_arn: $EKS_ROLE_ARN
        with:
          args: |
            kubectl set image deployment $K8S_DEPLOYMENT -n $K8S_NAMESPACE
            $K8S_DEPLOYMENT=$DOCKER_IMAGE:$DOCKER_TAG &&
            kubectl rollout status deployment/$K8S_DEPLOYMENT -n $K8S_NAMESPACE
