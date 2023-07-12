# School App Example with ArgoCD and Hardcoded Secrets

In this lab we will see how to use a GitOps approach with ArgoCD to run our school app with hardcoded secrets.

## Delete the Current School App

```bash
kubectl delete ns schoolapp
```

## Create the School App Application in ArgoCD

```bash
kubectl apply -f argocdSchoolApp.yaml
```

## Test the School App with Hardcoded Secrets

```bash
# Port forward the frontend
kubectl port-forward service/frontend 8001:8080 -n schoolapp
# Port forward the api
kubectl port-forward service/api 5000:5000 -n schoolapp
```

Try creating and deleting a course and check the logs of the api pod.

```bash
kubectl logs -n schoolapp -f $(kubectl get pods --template '{{range .items}}{{.metadata.name}}{{end}}' --selector=app=api) -c api
```

Notice this output:

**Expected Output**
```
MongoDB Credentials Using Hardcoded Values that appear in GitLab: 
Username = schoolapp
Password = mongoRootPass
```

This was using the hardcoded values, next let's try with the injector

> This concludes this lab