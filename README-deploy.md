## Create a Kubernetes Deployment

#### Helm

Go to the helm chart directory `cd charts/`.
1. Make sure the provided values are all correct in `values.yaml`.
2. Install dependencies <br/>
`helm dependency update ./charts` <br/>
    We use a pre-built Helm chart for the postgres database.
3. Create a release <br/>
`helm install todo-release ./charts`
4. To uninstall release use: <br/>
`helm uninstall todo-release`
