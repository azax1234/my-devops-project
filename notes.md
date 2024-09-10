### Notes 
Right now testing with Explore California website image that I have in my docker hub.

Intial Helm charts are for Explore California app.

Netflix-clone project will be done separately with its own charts and Argocd will deply this app as well into the same GKE cluster.

***ISSUES / OPEN QESTIONS / TO BE DONE***

1. Getting to many redirects error when  trying to se an ingress for argo cd without a host.

2. How to have multiple ingresses for differnt apps with same ingress controller. Do we have to separate them by sing host in spec or anything else. Using seprate controllers is an option?
.

***Github Actions***

1. How to make the workflow ignore ccertain commits push. Dont want it to build everytime.

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

***[INVALID] Skipping commit*** as it doesn't follow conventional commit format.
Error: No commit resulted in a version bump since last release!

Answer
I understand the issue you're facing. This error occurs when the ietf-tools/semver-action@v1 action can't find any commits that follow the Conventional Commits format since the last release. To address this, we have a few options:
Ensure your commits follow Conventional Commits format
Adjust the action to accept non-conventional commits
Use a different action that's more flexible
Let's go through these options:
Use Conventional Commits:
Ensure your commit messages follow the format: type(scope): description
For example: feat: add new feature or fix: resolve bug in login
****Conventional git commit format****
Structure:
Use the format: <type>(<optional scope>): <description>
Optionally add a body and footer separated by blank lines
Types:
feat!: Major release
feat: New feature
fix: Bug fix
docs: Documentation changes
style: Code style changes (formatting, missing semicolons, etc)
refactor: Code refactoring
perf: Performance improvements
test: Adding or updating tests
build: Build system or external dependency changes
ci: CI configuration changes

Adjust the semver action:
You can modify the action to treat all commits as patch bumps:
text
- name: Calculate SemVer
  id: semver
  uses: ietf-tools/semver-action@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    branch: main
    patchAll: true

Use a different action:
An alternative is to use anothrNick/github-tag-action@1.67.0, which is more flexible:
text
- name: Bump version and push tag
  id: tag_version
  uses: anothrNick/github-tag-action@1.67.0
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    DEFAULT_BUMP: patch
    WITH_V: true

Implement custom versioning logic:
If you need more control, you can implement your own versioning:
text
- name: Calculate SemVer
  id: semver
  run: |
    LATEST_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "v0.0.0")
    NEXT_VERSION=$(echo $LATEST_TAG | awk -F. '{$NF = $NF + 1;} 1' | sed 's/ /./g')
    echo "next=$NEXT_VERSION" >> $GITHUB_OUTPUT

Choose the option that best fits your project's needs:
If you want to stick with Conventional Commits, option 1 is best.
For a quick fix with the current action, use option 2.
For more flexibility without changing commit styles, consider options 3 or 4.
Remember to adjust subsequent steps in your workflow to use the correct output variable based on the method you choose.

