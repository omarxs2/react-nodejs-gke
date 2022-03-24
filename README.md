# react-nodejs-gke
Example Project on how to deploy React App with Nodejs Backend on GCP GKE

# Steps
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

4- Create a simple trigger on cloud build whenever you push on main branch (sample code trigger.json) 

5- Make any change on your code and push from localhost (like vscode) and the trigger will handle next steps

# Automated steps

on ``` cloudbuild.yaml ``` file, we are handling 3 steps:

1- Building the image.

2- Publishing (saving) the image to container registery

3- Deploying the image on kubernetes with manifest.yml file configretion.
at deploymet step we:
* assgning project id
* getting cluster config
* updating the image path on manifest.yml file using sed command
* running manifest.yml using kubectl
    