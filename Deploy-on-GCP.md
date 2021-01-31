## Deploy on Google Cloud Platform:

#### Build Images:
Using the google cloud console, run the following commands.

1. Go to the backend repository: ```[cd django-todo-react/backend]```
2. Run: ```[gcloud builds submit --tag gcr.io/PROJECT_ID/IMAGE_NAME:TAG]```
3. Go to the frontend repository: ```[cd ../frontend]```
4. Run: ```[gcloud builds submit --tag gcr.io/PROJECT_ID/IMAGE_NAME:TAG]```

#### Set correct Values
5. Go to the charts repository ```[cd ../charts]```
6. Edit the values.yaml file: ```[nano values.yaml]```
7. Fill all instances of 'image' with the correct PROJECT_ID: ```[image: gcr.io/[PROJECT_ID]/[NAME]:latest]```

#### Install helm chart
8. Return to main repository: ```[cd ../]```
9. Run: ```[Helm install todo-release ./charts/]```
10. Wait around 5 minutes for the cloud to configure the ingress resource for the application.
11. Retrieve the IP adress from the ingress resource: ```[kubectl get ingress]```
12. Enter IP into browser, enjoy the application :)
