### Notes 
***(To be arranged)***
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "ClusterIP"}}'

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

helm upgrade --atomic --install explore-california-website ./chart -f ./chart/values-dev.yaml

***ISSUES / OPEN QESTIONS***

1. Getting to many redirects error when  trying to se an ingress for argo cd without a host.

2. How to have multiple ingresses for differnt apps with same ingress controller. Do we have to separate them by sing host in spec or anything else. Using seprate controllers is an option?

