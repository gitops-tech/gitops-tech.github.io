## Getting Started with GitOps on Google Cloud

This page explains how to set up pull-based GitOps on Google Cloud Platform using Weavework's [flux](https://github.com/fluxcd/flux).
All code used on this page can be found in full on [GitHub](https://github.com/gitops-tech).

### Setting Up the Google Cloud Project

Either select an existing project or [create a new one](https://console.cloud.google.com/cloud-resource-manager) in the Google Cloud Console.

For this project, the Google Kubernetes Engine and Google Cloud Build APIs need to be enabled.

![Setting up Google Kubernetes Engine API](images/cs-apis.gif)



### Building Container Images with Google Cloud Build

Google Cloud Build uses the `cloudbuild.yaml` file in the root of the repository to find the steps to execute the build.
For this project, first the Go binaries need to be build.
Then, the container image is build, tagged and pushed to the Google Container Registry.
The following `cloudbuild.yaml` file can also be found on [GitHub](https://github.com/gitops-tech).

```yaml
steps:
# build the binaries
- name: 'gcr.io/cloud-builders/go'
  args: ['build', '-o', 'hello', 'src/main.go']

# build the container image, tag it and push it to the Google Container Registry of our project
- name: 'gcr.io/cloud-builders/docker'
  args: ['build', 
      '-t', 'gcr.io/$PROJECT_ID/$REPO_NAME:$BRANCH_NAME-$SHORT_SHA',
      '-t', 'gcr.io/$PROJECT_ID/$REPO_NAME:latest',
      '-f', 'Dockerfile', '.']

images:
- 'gcr.io/$PROJECT_ID/$REPO_NAME:$BRANCH_NAME-$SHORT_SHA'
- 'gcr.io/$PROJECT_ID/$REPO_NAME:latest'
```

Next, a trigger for Google Cloud Build needs to be created, to execute the build when new commits are pushed to the repository.

![Setting up the Google Cloud Build Trigger on the Application Repository](images/cs-trigger.gif)

### Adding the Kubernetes Manifests to the Environment Repository

* Write YAMLs
* push to env repo

### Deploying the Operator

* clone flux
* edit its configuration
* deploy to cluster