```
# create new org "demo"
# create new repo "quarkus-app" in demo org
# create new "rh-openjdk-11-runtime" in "demo" org for mirroring registry.access.redhat.com/ubi8/openjdk-11-runtime 

# create robot account "ci" with write access to "quarkus-app" and read access on "rh-openjdk-11-runtime"
# download k8s secret containing "ci" robot account credentials and replace the .dockerconfigjson field in quay-auth-secret.yaml

# go to repo settings on "rh-openjdk-11-runtime" and change Repository State to Mirror
# go to mirror config and add remote repo to be mirrored and click on "Enable mirror"
  location: registry.access.redhat.com/ubi8/openjdk-11-runtime
  tags: *
  start date: now!
  sync interval: 1 days
  robot user: demo+ci 
  verify TLS: check
# Click on Sync Now to sync manually 


# create build pipeline
$ oc new-project demo
$ oc create -f secrets/quay-auth-secret.yml
$ oc apply -f secrets/pipeline-sa.yaml
$ oc create -f pipeline
$ oc create -f triggers

# go to console pipeline details page and copy webhook url
# go to quay and add notification on push to repository with POST to the webhook url
```