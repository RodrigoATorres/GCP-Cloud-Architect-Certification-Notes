# Container Registry
https://cloud.google.com/container-registry/docs/overview

- Private container image registry that runs on Google Cloud
- Docker Image Manifest V2 and OCI image formats
- to control access to your images you need to use a private registry such as Container Registry (not Docker Hub)
- access Container Registry through secure HTTPS endpoints
  - push, pull, and manage images from any system, VM instance, or your own hardware

- Integrate Container Registry with Google Cloud CI/CD services or your existing CI/CD tools.
  - Store artifacts from Cloud Build.
  - Deploy artifacts to Google Cloud runtimes, including Google Kubernetes Engine, Cloud Run, Compute Engine, and App Engine flexible environment.
  - Identity and Access Management provides consistent credentials and access control.
- Secure your container software supply chain.
  - Manage container metadata and scan for container vulnerabilities with Container Analysis.
  - Enforce deployment policies with Binary Authorization.
- Protect the registry in a VPC Service Controls security perimeter.

## Registries

- named by host and project ID
  - HOSTNAME/PROJECT-ID/IMAGE:TAG or HOSTNAME/PROJECT-ID/IMAGE@IMAGE-DIGEST
    - HOSTNAME is the location where the image is stored (gcr.io, us.gcr.io etc..)
    - adding either :TAG or @IMAGE-DIGEST at the end allows you to distinguish a specific version of the image, but it is also optional

## Organizing images with repositories

- You can group related images under a repository within a registry
- When you tag, push, or pull an image, you specify the repository name under the project in the image path.
  - us.gcr.io/builds/dev/web-app:beta-2.0
  - us.gcr.io/builds/stable/web-app:1.0
- If needed, you can nest repositories
  - us.gcr.io/builds/product1/dev/product1-app:beta-2.0
  - us.gcr.io/builds/product1/stable/product1:1.0
  - us.gcr.io/builds/product2/dev/product2:alpha
  - us.gcr.io/builds/product2/stable/product2:1.0

## Versions of images within a registry
- A registry can contain many images, and these images may have different versions
- To identify a specific version of the image within a registry, you can specify the image's tag or digest.

## Domain-scoped projects

If your project is scoped to your domain, the project ID includes the name of the domain followed by a colon ( : ). Because of how Docker treats colons, you must replace the colon character with a forward slash when you specify an image digest in Container Registry.
- gcr.io/example.com/my-project/image-name

## Registry names as URLs
- The URL https://HOSTNAME/PROJECT-ID/IMAGE is a URL for that registry in the Cloud Console
- can be visited by any authenticated user who has permission

## Access control

- Container Registry stores its tags and layer files for container images in a Cloud Storage bucket
- Access to the bucket is configured using Cloud Storage's IAMs

## Authentication

- You can configure Docker to use the gcloud command-line tool to authenticate requests to Container Registry
- also supports advanced authentication methods using access tokens or JSON key files

## Container Registry service account

- service-[PROJECT_NUMBER]@containerregistry.iam.gserviceaccount.com
- for Container Registry to perform its service duties on your project
- Google owns this account
- If you delete this service account or change its permissions, certain Container Registry features will not work correctly. You should not modify roles or delete the account.

## Pull-through cache

- mirror.gcr.io registry caches frequently requested public images
- Using cached images can speed up pulls from Docker Hub
- Your client always checks for a cached copy 

## Notifications

- You can use Pub/Sub to get notifications about changes to your container images.

## Using Container Registry with Google Cloud

- Compute Engine instances and Google Kubernetes Engine clusters can push and pull Container Registry
- can be deployed to the App Engine flexible environment

## Continuous delivery tool integrations

- Container Registry works with several popular continuous delivery systems

## Using Container Registry with third-party solutions

- Container Registry can be integrated  whit third-party cluster managment, continuous integration or other solutions outside of Google Cloud
https://cloud.google.com/container-registry/docs/continuous-delivery
