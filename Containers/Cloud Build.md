# Cloud Build

- service that executes your builds on Google Cloud Platform's infrastructure
  -  import source code from a variety of repositories or cloud storage spaces
  -  execute a build to your specifications
  -  produce artifacts such as Docker containers or Java archives

### Build configuration and build steps

- build config -> provide instructions to Cloud Build on what tasks to perform
- builds to:
  - fetch dependencies
  - run
    - unit tests
    - static analyses
    - integration tests
  - create artifacts with build tools such
    - docker
    - gradle
    - maven
    - bazel
    - and gulp
- executes your build as a series of build steps, where each build step is run in a Docker container
  - analogous to executing commands in a script
  - use the build steps provided by Cloud Build, Community-contributed build steps or your own custom
  - Each build step is run with its container attached to a local Docker network named cloudbuild
    - This allows build steps to communicate with each other and share data

### Starting builds

- You can
  - manually start builds in Cloud Build using the gcloud command-line tool or the Cloud Build API
  - use Cloud Build's build triggers feature
    - create an automated continuous integration/continuous delivery (CI/CD) workflow
    - starts new builds in response to code changes
  - integrate build triggers with many code repositories, including Cloud Source Repositories, GitHub, and Bitbucket
### Viewing build results
  - view your build results using the gcloud tool, the Cloud Build API or use the Build History page
     - details and logs for every build Cloud Build executes

### How builds work

- lifecycle of a Cloud Build build:
1. Prepare your application code and any needed assets.
1. Create a build config file in YAML or JSON format, which contains instructions for Cloud Build.
1. Submit the build to Cloud Build.
1. Cloud Build executes your build based on the build config you provided.
1. If applicable, any built images are pushed to Container Registry. Container Registry provides secure, private Docker image storage on Google Cloud.
- Cloud Build uses Docker to execute builds

### Cloud Build interfaces

- you can use
  - Google Cloud Console
  - gcloud command-line tool
  - Cloud Build's REST API

### Running builds locally
 
- If you want to test your build before submitting it to Cloud Build, you can run your build locally using the cloud-build-local tool

## Deploying to Cloud Run

- Cloud Run is a managed compute platform that enables you to run stateless containers in a serverless environment
- automatically deploy Cloud Run services using Cloud Build
- Cloud Build enables you to
  - build the container image
  - store the built image in Container Registry
  - deploy the image to Cloud Run.]
- create a config file named cloudbuild.yaml:
  ```yaml
  steps:
  # Build the container image
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t', 'gcr.io/PROJECT_ID/IMAGE', '.']
  # Push the container image to Container Registry
  - name: 'gcr.io/cloud-builders/docker'
    args: ['push', 'gcr.io/PROJECT_ID/IMAGE']
  # Deploy container image to Cloud Run
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    entrypoint: gcloud
    args: ['run', 'deploy', 'SERVICE-NAME', '--image', 'gcr.io/PROJECT_ID/IMAGE', '--region', 'REGION', '--platform', 'managed']
  images:
  - gcr.io/PROJECT_ID/IMAGE
  ```
  Where:
  - SERVICE-NAME is the name of the Cloud Run service.
  - REGION is the region of the Cloud Run service you are deploying.
  - PROJECT_ID is your Google Cloud project ID where your image is stored.
  - IMAGE is the name of your image in Container Registry.
-  `gcloud builds submit`

### Continuous deployment

- can automate the deployment of your software to Cloud Run by creating Cloud Build triggers

## Where to deploy on

- GKE.
- Cloud Functions.
- App Engine.
- Firebase.