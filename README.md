# kubernetes-dashboard
#menggunakan kops
#domain terlebih dahulu
#s3 bucket 
==========
#cek ns domain
dig ns (..)
======install awscli=====
sudo apt-get update
sudo apt-get install awscli
aws configure
======install kubectl=====
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
======install kops========
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
=======expose env dari s3===========
export KOPS_STATE_STORE=s3://(....)
=======membuat cluster=========
kops create cluster --node-count=3 --node-size=t2.micro --master-size=t2.micro --zones=(...) --name kubernetes.(...)
=======optional apabila error===========
ssh-keygen -t rsa
========edit cluster============
kops edit cluster --name(..)
========build cluster==========
kops update cluster (..) --yes --admin
======pointing ip(memasukan ip domain api)===========
sudo vim /etc/hosts
==========cek keberhasilan=========
kops validate cluster
=========download akses dashboard========
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
==========kubectl port-forward==========
kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8080:443
========akses ke dashboard=======
https://localhost:8080
