# do-k8s-deploy

Action to apply a deployment to a Digital Ocean Kubernetes cluster.

## k8s yaml example - '/iac/deployment.yaml'
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-name
  namespace: namespace-name
  annotations:
    github-action-id: <ACTION-ID> #action-id to track action in github
spec:
  selector:
    matchLabels:
      app: app-name
  replicas: 1
  template:
    metadata:
      labels:
        app: app-name
    spec:
      containers:
      - name: app-name
        image: your-org/your-image:<ID>   #tag-name parameter will be replaced by <ID>
                
```

## Github action code example

```
name: CI-DO

on:
  push:
    branches: [ "*" ]
  release:
    types: [created]

jobs:
  create-image-and-deploy-hml:
    name: Deploy
    runs-on: ubuntu-latest

    if: github.ref == 'refs/heads/main'

    steps:

        ...
      - name: Deploy HML
        id: deploy_hml
        uses: agro1inovacao/do-k8s-deploy@v6
        with:
          tag-name: 'staging' # the tag-name of image, can be created dinamically too
          do-token: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }} # create a secret with your token
          cluster-name: 'k8s'
          yaml-file: '/iac/deployment.yaml'
          app-name: 'app-name'
          namespace: 'namespace-name'
          type: 'deployment'
```
