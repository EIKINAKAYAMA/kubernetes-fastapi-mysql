# Study Kubernetes

<img width="1048" alt="スクリーンショット 2023-04-27 14 54 51" src="https://user-images.githubusercontent.com/65437818/234771904-9cd18dca-7cb2-407d-b903-b9c6c704ff62.png">

# Process

### create db cluster
1. create database ns
~~~ sh
kubectl create namespace database
~~~


2. create deployment for mysql
~~~ sh
kubectl apply -f mysql-deployment.yaml -n database
~~~

### create app cluster
Image base
https://github.com/tiangolo/uvicorn-gunicorn-fastapi-docker

1. create app ns
~~~ sh
kubectl create namespace $namespace
~~~

2. apply deployment, configMap, secret
~~~ sh
kubectl apply -f sample-app-deployment.yaml,sample-app-configmap.yaml,sample-app-secret.yaml -n $namespace
~~~

### Access from local
~~~ sh
kubectl port-forward svc/sample-app 8080:80 -n $namespace
~~~

# Tips
- apiVersion の確認方法

Ex.kind: deployment
~~~ sh
kubectl api-resources | grep deployments
~~~

-  imageを指定して --dry-run オプションで作成されるオブジェクトをyamlファイルに出力
~~~ sh
kubectl careate deploy mysql --image=mysql:8 --dry-run=client -o yaml > mysql-deployment.yaml
~~~
   
- localでアプリへの接続を確認
~~~ sh
kubectl port-forward svc/sample-app 8080:80 -n $namespace
~~~

- コンテナ内に入って確認する（Ex.mysql）
~~~ sh
kubectl get po -n database | grep mysql | head -1 | awk '{print $1}' // $container
kubectl exex -it $container --mysql -uroot -ppassword
~~~
 
