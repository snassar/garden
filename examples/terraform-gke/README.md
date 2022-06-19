# Terraform + GKE example

This example takes the [demo project](../demo-project) and sets it up for use with a GKE cluster, using Terraform.

## Setup

### Step 1 - Install the necessary software

#### Install the Google Cloud SDK and authenticate

If you haven't already, follow the [google cloud SDK quickstarts](https://cloud.google.com/sdk/docs/quickstarts) instructions to install the `gcloud` tool.

Now authenticate with GCP:

```sh
gcloud auth application-default login
```

#### kubectl

If you haven't already, follow the [kubernetes tools: kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) instructions to install the `kubectl` tool.

#### garden

If you haven't already, follow the [garden.io installation](https://docs.garden.io/getting-started/1-installation) instructions to install the `garden` tool.

#### terraform

If you haven't already, follow the [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform) instructions to install the `terraform` tool.

#### docker

If you haven't already, follow the [Install Docker Engine](https://docs.docker.com/engine/install/) instructions to install the `docker` tool.

### Step 2 - Set up a GCP project

Choose a project ID for the demo project and run the following (skip individual steps as appropriate):

```sh
export PROJECT_ID=<id>
# (Skip if you already have a project)
gcloud projects create $PROJECT_ID
# If you haven't already, enable billing for the project (required for the APIs below).
# You need an account ID (of the form 0X0X0X-0X0X0X-0X0X0X) to use for billing.
gcloud alpha billing projects link $PROJECT_ID --billing-account=<account ID>
# Enable the required APIs (this can sometimes take a while).
gcloud services enable compute.googleapis.com container.googleapis.com servicemanagement.googleapis.com artifactregistry.googleapis.com --project $PROJECT_ID
```

### Step 3 - Set your stack variables

The Terraform stack needs some input values from you. You can provide those in a couple of different ways:

**A)** Supply the values in a `terraform.tfvars` file in the example root directory. For example:

```tfvars
# terraform.tfvars
gcp_region     = "europe-west1"°
gcp_project_id = "my-test-project"
```

**B)** Configure the variables directly in the project `garden.yml` (see the `terraform` provider section).

### Step 4 - Initialize the cluster

Install the cluster-wide services Garden needs by running:

```sh
garden plugins kubernetes cluster-init
```

This will take a while because the cluster needs to be provisioned, and some services installed when it's ready.

### Step 5 - Deploy your services

Finally, to build and deploy your services to your new GKE cluster, run:

```sh
garden deploy
```

And that's it!

## Cleanup

Simply delete your GCP project, and the Terraform state:

```sh
gcloud projects delete $PROJECT_ID
rm -rf .terraform terraform.tfstate
```
