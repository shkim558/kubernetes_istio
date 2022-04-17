FROM ubuntu:20.04

WORKDIR /app

RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install sudo

## install aws cli
RUN apt-get install -y unzip curl vim less ssh snapd
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
RUN sudo ./aws/install
RUN mkdir ~/.aws

## install eksctl
RUN curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
RUN sudo mv /tmp/eksctl /usr/local/bin

## install kubectl
RUN curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
RUN chmod +x ./kubectl
RUN mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
RUN echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc

## install istio
RUN curl -sL https://istio.io/downloadIstioctl | sh -
RUN cp ~/.istioctl/bin/istioctl ~/bin

## install helm
RUN curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
RUN sudo apt-get install apt-transport-https --yes
RUN echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
RUN sudo apt-get update -y
RUN sudo apt-get install -y helm

## add eks to helm repository
RUN helm repo add eks https://aws.github.io/eks-charts
RUN helm repo update
