# Kubernetes 공부 

- https://basketdeveloper.tistory.com/entry/Kubernetes-ArgoCD-%EC%84%A4%EC%B9%98-%ED%95%B4%EB%B3%B4%EA%B8%B0with-Minikube

## Minikube 
- minikube start


## ArgoCD
-  kubectl create namespace argocd
-  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
-  kubectl get all -n argocd
-  kubectl port-forward svc/argocd-server -n argocd 8080:443
-  kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

## Ingress Add on
- minikube addons enable ingress
- kubectl get po -n ingress-nginx
- minikube service ingress-nginx-controller -n ingress-nginx --url (test)

## materic add on
- minikube addons enable dashboard
- minikube addons enable metrics-server

## Kubeflow
- kubectl create namespace kubeflow
- (optional) git clone https://github.com/kubeflow/manifests.git 
- create argoCD Yaml.
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kubeflow
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/kubeflow/manifests'
    targetRevision: master
    path: example
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: kubeflow
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```
- kubectl apply -f kubeflow-app.yaml
- argocd app get kubeflow