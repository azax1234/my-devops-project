### Notes 
Right now testing with Explore California website image that I have in my docker hub.

Intial Helm charts are for Explore California app.

Netflix-clone project will be done separately with its own charts and Argocd will deply this app as well into the same GKE cluster.

***(To be arranged)***
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "ClusterIP"}}'

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

helm upgrade --atomic --install explore-california-website ./chart -f ./chart/values-dev.yaml

**semVar**

Error: Couldn't find the latest tag. Make sure you have at least one tag created first!

This error occurs when the ietf-tools/semver-action@v1 action can't find any existing tags in your repository to use as a base for calculating the next semantic version. To resolve this issue, you have a few options:
Create an initial tag:
If your repository doesn't have any tags yet, you need to create an initial tag. You can do this manually:
bash
git tag v0.1.0
git push origin v0.1.0

Modify the semver action to handle no tags:
You can adjust the semver action to start from a default version if no tags are found. Update your workflow like this:
text
- name: Calculate SemVer
  id: semver
  uses: ietf-tools/semver-action@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    branch: main
    default-version: '0.1.0'

The default-version parameter tells the action what version to use if no tags are found.

***ISSUES / OPEN QESTIONS / TO BE DONE***

1. Getting to many redirects error when  trying to se an ingress for argo cd without a host.

2. How to have multiple ingresses for differnt apps with same ingress controller. Do we have to separate them by sing host in spec or anything else. Using seprate controllers is an option?

3. learn about how to maintain RLEASES in Helm charts. There seems to be a  RELEASE variable that we can maintain.

