# Cloud Deployment Manager

- Repeatable deployment process
  - Treat your configuration as code and perform repeatable deployments
- Declarative language
  - specify all the resources needed for your application in a declarative format using yaml
- Focus on the application
- Template-driven
  - use Python or Jinja2 templates to parameterize the configuration and allow reuse of common deployment paradigms such as a load balanced, auto-scaled instance group
- Parallel deployment
- Updates
- Input and output parameters
  - Pass variables
    - e.g. zone, machine size, number of machines, state: test, prod, staging 
  - get output values back
    - e.g. IP address assigned, link to the instance
- Schema files
  - JSON schema for defining and constraining parameters.
- Preview mode
  - See what changes Deployment Manager will make on a create or update operation before you commit the changes
- References
  - One resource definition can reference another resource creating dependencies and controlling the order of resource creation

## Deployment Manager Fundamentals

### Configuration

- describes all the resources you want for a single deployment
    ```yaml
    resources:
    - name: the-first-vm
    type: compute.v1.instance
    properties:
    zone: us-central1-a
    machineType: https://www.googleapis.com/compute/v1/projects/myproject/zones/us-central1-a/machineTypes/f1-micro
    disks:
    - deviceName: boot
        type: PERSISTENT
        boot: true
        autoDelete: true
        initializeParams:
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-7-wheezy-v20150423
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/myproject/global/networks/default
        accessConfigs:
        - name: External NAT
        type: ONE_TO_ONE_NAT
    ```

### Templates

- A configuration can contain templates
  - Python or Jinja2

Configuration file using templates:
```yaml
imports:
- path: vm_template.jinja

resources:
- name: vm-instance
  type: vm_template.jinja
  properties:
    zone: us-central1-a
    project: myproject
```

### Resource

- A resource represents a single API resource
  -  This can be an API resource provided by a Google-managed base type or an API resource provided by a Type Provider

### Types
https://cloud.google.com/deployment-manager/docs/configuration/supported-resource-types

- A type can represent:
  - a single API resource, known as a base type
    - supported by a RESTful API that supports Create, Read, Update, and Delete (CRUD) operations
    - You can also create additional base types by adding a type provider if the Google-owned types alone do not meet your needs (Creating your own type provider is an advanced scenario, and Google recommends that you do this only if you are very familiar with the API you want to integrate)
      - supply an API descriptor document, which can be an OpenAPI specification or a Google Discovery
      - adjust any necessary input mappings for the API
      - register the type with Deployment Manager
  - a set of resources, known as a composite type
  - syntaxes:
    - `type: [API].[VERSION].[RESOURCE]`
    - `type: gcp-types/[PROVIDER]:[RESOURCE]`
    - `type: [PROJECT_ID]/[TYPE_PROVIDER]:[COLLECTION]`

### Composite types

- contains one or more templates that are preconfigured to work together
- expand to a set of base types when deployed in a deployment
- essentially hosted templates that you can add to Deployment Manager
- You can create composite types for common solutions so that the solution is easily reusable, or create complex setups that you can reuse in the future

### Manifest

- a read-only object that contains the original configuration you provided
  - including any imported templates
  - also contains the fully-expanded resource list, created by Deployment Manager
- Each time you update a deployment, Deployment Manager generates a new manifest file to reflect the new state of the deployment
- usefull for troubleshooting

### Deployment
A deployment is a collection of resources that are deployed and managed together, using a configuration.