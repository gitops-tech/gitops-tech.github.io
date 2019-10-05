## Getting Started with GitOps on Google Cloud

This page explains how to set up pull-based GitOps on Google Cloud Platform using Weavework's [flux](https://github.com/fluxcd/flux).

### Setting Up the Google Cloud Project

* Create Project
* Enable APIs (GKE, GCB, GCR)

  ![Setting up Google Kubernetes Engine API](images/cs-apis.gif)

* Create Cluster with TF

### Building Container Images with Google Cloud Build

* Write cloudbuild.yaml
* Set up GCB trigger on GitHub repo

### Adding the Kubernetes Manifests to the Environment Repository

* Write YAMLs
* push to env repo

### Deploying the Operator

* clone flux
* edit its configuration
* deploy to cluster