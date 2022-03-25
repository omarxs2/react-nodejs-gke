# react-nodejs-gke
Example Project on how to deploy React App with Nodejs Backend on GCP GKE

# Mannual Steps
1- Creat your own vpc (my case omar-vpc)

2- Create containter cluster
``` 
gcloud container clusters create omar-cluster \
    --project=devops-343007 \
    --zone=us-west2-a \
    --network=omar-vpc
} 
```

3- Miror your gihub code to Google Cloud Repos

4- Create a simple trigger on cloud build whenever you push on main branch (sample code at ``` trigger.json ```) 
```
gcloud beta builds triggers create cloud-source-repositories \
  --trigger-config trigger.json
  ```

5- Make any change on your code and push from localhost (like vscode) and the trigger will handle next steps, to push:

``` change anything in the code ```

``` git add . ```

``` git commit -m "new commit" ```

``` git push ```

# Automated Steps

On ``` cloudbuild.yaml ``` file, we are handling 3 steps:

1- Building the image.

2- Publishing (saving) the image to container registery

3- Deploying the image on kubernetes with manifest.yml file configretion.

At deploymet step we are:

* getting cluster config

``` 
gcloud container clusters get-credentials "omar-cluster" \
        --project "devops-343007" \
        --zone "us-west2-a" 
 ```

* updating the image path on manifest.yml file using sed command

``` sed -i 's|gcr.io/devops-343007/omar-react-image:.*|gcr.io/devops-343007/omar-react-image:${SHORT_SHA}|' manifest.yml ```

* running manifest.yml using kubectl

``` kubectl apply -f manifest.yml ```
    
