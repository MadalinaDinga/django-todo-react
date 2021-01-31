- Show how you build the container images and publish to a registry. Show how you deploy the application. Show how to scale the application horizontally (stateless parts only). Show how to uninstall the application.
	- comparison Helm vs kubectl apply
	- dashboard


######### build + publish

cd app/backend
sudo docker build -t todo-api .
sudo docker tag todo-api localhost:32000/todo-api
sudo docker push localhost:32000/todo-api

cd
cd app/frontend
sudo docker build -t todo-ui .
sudo docker tag todo-ui localhost:32000/todo-ui
sudo docker push localhost:32000/todo-ui

sudo docker images

cd ..
microk8s helm3 dependency update ./charts
microk8s helm3 install todo-release ./charts/


######### install helm chart

cd app
microk8s helm3 dependency update ./charts
microk8s helm3 install todo-release ./charts
microk8s helm3 list
microk8s helm3 uninstall todo-release


######### kubectl apply instalation

cd
cd app
kubectl apply -f ./charts/templates/ingress.yaml
kubectl apply -f ./charts/templates/tls-secret.yaml
kubectl apply -f ./charts/templates/todo-api-deployment.yaml
kubectl apply -f ./charts/templates/todo-api-service.yaml
kubectl apply -f ./charts/templates/todo-lb.yaml
kubectl apply -f ./charts/templates/todo-ui-deployment.yaml
kubectl apply -f ./charts/templates/todo-ui-service.yaml
kubectl apply -f ./charts/templates/todo-api-deployment.yaml


######### autoscale

kubectl autoscale deployment todo-ui-deployment --min=4 --max=5 --cpu-percent=60
kubectl delete hpa


######### istio 

cd
cd app_istio
kubectl label namespace default istio-injection=enabled
microk8s helm3 dependency update ./charts
microk8s helm3 install todo-release ./charts
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')




