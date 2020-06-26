# hello-python
Hello World in Python

## How to use

Build the image:
```
docker login --username=yourhubusername --password=yourpassword

docker build -t yourhubusername/hello-python:v1 .

docker push yourhubusername/hello-python:v1
```

Test locally: 
```
docker run -d --name hello-python -p 8080:5000 jonjozwiak/hello-python:v1
sleep 2
curl http://localhost:8080

docker stop hello-python; docker rm hello-python
```

Deploy to kubernetes: 
```
kubectl apply -f k8s-deployment.yaml

kubectl get pods 

kubectl get service hello-python-service
curl http://<URL_from_Service>
## Note - It may take a minute or two to resolve DNS

kubectl get deployments
```

## Push an update and rollback 

```
vi app/main.py
# Change the text to include something like v2 and build a new image 

docker build -t yourhubusername/hello-python:v2 .
docker push yourhubusername/hello-python:v2
```

Now deploy your new image

```
kubectl set image deployment/hello-python hello-python=yourhubusername/hello-python:v2 --record

kubectl get deployments 
curl http://<URL_from_Service>
```

Now rollback to a previous version

```
kubectl rollout history deployment/hello-python
kubectl rollout undo deployment/hello-python --to-revision=1

kubectl get deployment
curl http://<URL_from_Service>
```

## Cleanup 

```
kubectl delete -f k8s-deployment.yaml
```

