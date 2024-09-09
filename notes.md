### Notes 
Right now testing with Explore California website image that I have in my docker hub.

Intial Helm charts are for Explore California app.

Netflix-clone project will be done separately with its own charts and Argocd will deply this app as well into the same GKE cluster.

***(To be arranged)***
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "ClusterIP"}}'

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

helm upgrade --atomic --install explore-california-website ./chart -f ./chart/values-dev.yaml

***ISSUES / OPEN QESTIONS***

1. Getting to many redirects error when  trying to se an ingress for argo cd without a host.

2. How to have multiple ingresses for differnt apps with same ingress controller. Do we have to separate them by sing host in spec or anything else. Using seprate controllers is an option?

