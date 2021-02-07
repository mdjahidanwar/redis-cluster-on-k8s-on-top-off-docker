#############Kind installation:############ 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.10.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /usr/local/bin/kind

###################redis cluster on k8s on top of docker ############
      ###creating a cluster and name place###
kind create cluster --name redis --image kindest/node:v1.18.4

kubectl create ns redis
kubect

###########################Deployment: Redis nodes################

kubectl apply -n redis -f redis-configmap.yaml
kubectl apply -n redis -f redis-statefulset.yaml

kubectl -n redis get pods
kubectl -n redis get pv

kubectl -n redis logs redis-0
kubectl -n redis logs redis-1
kubectl -n redis logs redis-2



##############################Test replication status###################
kubectl -n redis exec -it redis-0 sh
redis-cli 
auth a-very-complex-password-here
info replication


##########################Deployment: Redis Sentinel (3 instances)#################
cd storage/redis/kubernetes/
kubectl apply -n redis -f ./sentinel/sentinel-statefulset.yaml

kubectl -n redis get pods
kubectl -n redis get pv
kubectl -n redis logs sentinel-0
kubectl -n redis delete pod sentinel-0
kubectl -n redis logs sentinel-0


Load banacing 
kubectl apply -n redis -f lb.yaml
