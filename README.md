# blue-green-cicd-architecture
Complete deployment process with CI/CD automation including blue/green deployment policy

## What is CI/CD?

CI/CD is a method to frequently deliver apps to customers by introducing automation into the stages of app development. The main concepts attributed to CI/CD are continuous integration, continuous delivery, and continuous deployment.

CI/CD is an essential part of DevOps and any modern software development practice. A purpose-built CI/CD platform can maximize development time by improving an organization’s productivity, increasing efficiency, and streamlining workflows through built-in automation, testing, and collaboration

## What is continuous testing? 

Continuous testing is a software testing practice where tests are continuously run in order to identify bugs as soon as they are introduced into the codebase. In a CI/CD pipeline, continuous testing is typically performed automatically, with each code change triggering a series of tests to ensure that the application is still working as expected. 

In continuous testing, various types of tests are performed within the CI/CD pipeline. These can include:

- Unit testing, which checks that individual units of code work as expected
- Integration testing, which verifies how different modules or services within an application work together
- Regression testing, which is performed after a bug is fixed to ensure that specific bug won't occur again


## What are some common CI/CD tools?
CI/CD tools can help a team automate their development, deployment, and testing. Some tools specifically handle the integration (CI) side, some manage development and deployment (CD), while others specialize in continuous testing or related functions.

One of the best known open source tools for CI/CD is the automation server Jenkins.
## What is a blue-green deployment?
Blue/green deployment is a software deployment approach that helps organizations deploy frequent updates while maintaining high quality and a smooth user experience.A blue-green deployment uses two production environments (known as Blue and Green) to provide reliable testing, continuous no-outage releases, and instant rollbacks.At any given time, only one of the production environments is live. 

The Blue environment, for example, receives all user traffic while the clone (Green) remains idle. When a new version of the application is ready for release, it is deployed to the Green environment for testing. Once the release passes testing, the application traffic is routed from Blue to Green. The Green environment becomes the new production environment, and the Blue goes idle.

<kbd>![ Blue/Green deployment ](./blue-green.png)</kbd>