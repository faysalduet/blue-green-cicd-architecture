# blue-green-cicd-architecture

CI/CD with blue-green deployment is a strategy for deploying applications in a way that minimizes downtime and risk. It involves maintaining two identical environments, one "blue" and one "green", with only one of them live at any given time.

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

Here are some popular CI/CD tools to consider:

**Jenkins:** Jenkins is one of the most widely used CI/CD tools, and it is open-source and free. It provides a vast number of plugins to support various platforms and technologies.

**CircleCI:** CircleCI is a cloud-based CI/CD tool that can be used to automate builds, tests, and deployments. It offers a simple user interface and easy configuration for building and testing your projects.

**GitLab CI/CD:** GitLab CI/CD is a complete DevOps platform that provides a built-in CI/CD pipeline. It can be used to build, test, and deploy code automatically.

**Travis CI:** Travis CI is a cloud-based CI/CD tool that can be used to build and test code on multiple platforms. It integrates well with GitHub, making it an ideal choice for open-source projects.

One of the best known open source tools for CI/CD is the automation server **Jenkins** .But **GitLab CI/CD** is a powerful and flexible tool that offers many benefits, especially if you're already using GitLab for version control.

## What is a blue-green deployment?

Blue/green deployment is a software deployment approach that helps organizations deploy frequent updates while maintaining high quality and a smooth user experience.A blue-green deployment uses two production environments (known as Blue and Green) to provide reliable testing, continuous no-outage releases, and instant rollbacks.At any given time, only one of the production environments is live.

The Blue environment, for example, receives all user traffic while the clone (Green) remains idle. When a new version of the application is ready for release, it is deployed to the Green environment for testing. Once the release passes testing, the application traffic is routed from Blue to Green. The Green environment becomes the new production environment, and the Blue goes idle.

<kbd>![ Blue/Green deployment ](./blue-green.png)</kbd>

### Challenge of blue-green deployment

The shared database presents a challenge: two versions of the application code must be running at the same time, communicating with the same database.

There are two requirements for implementing blue-green deployments using a shared database:

- **Make all database changes backward compatible.**
  Any changes made to the database schema must not break the existing application.
- **Decouple database changes from application changes.**
  Since all database changes are backward compatible, they do not have to be deployed at the same time as the application changes. The database changes can be made ahead of time so that the database can serve either Blue or Green environments.

### Solving the database issue

Deploying changes to the shared database requires a coordinated and gradual approach. The database state is migrated with many small steps to the new state without any downtime. This process requires a level of precision and control that is difficult to achieve by manually deploying scripts.

Let’s look at a specific example. We want to rename a column from ZIP_CODE to POSTAL_CODE to accommodate our international customers. Assume that “Blue” is currently online.

- DB Change 1: Use migration to add the column POSTAL_CODE
- App Change 1: Update Green with an application version that reads - ZIP_CODE and writes to both ZIP_CODE and POSTAL_CODE.
- Router Change 1: Green goes online.
- DB Change 2: Deploy a script to transfer all values from ZIP_CODE to POSTAL_CODE.
- App Change 2: Update Blue with an application version that reads and writes only the new column POSTAL_CODE.
- Router Change 2: Blue goes online. Now only the new column POSTAL_CODE is used.
- DB Change 3: Deploy a script to remove the old column ZIP_CODE from the table.

### Common database change scenarios

There are several other database change scenarios detailed in the list below. For each scenario, I’ve included which sections could be applied to the scenario.

- **Adding a new column:** Follow all the steps except deleting old columns and handling legacy columns.
- **Renaming a column:** Don’t rename. Follow the steps above to add a new column and remove the old column.
- **Adding a new table:** Similar to adding a new column.
- **Renaming a table:** Don’t rename. Follow the steps above to add a new column and delete the old columns.
- **Moving a column to another table:** Very similar to adding a new column and removing the old column. The only difference is where the column was added.
- **Deleting a table:** Very similar to deleting a column. Hopefully, that table isn’t used anymore, and all the data has been migrated to other tables.
- **Adding a new view/stored procedure:** Very similar to adding a new column. The updated code will use the new view/stored procedure; the old code will not use the view/stored procedure.
- **Updating an existing view/stored procedure:** As long as columns aren’t removed from a view, this should be fine. If columns are removed, then the process to delete columns from above should be followed.
- **Removing a view/stored procedure:** Very similar process to removing columns. Except there won’t be data, just references in code and potentially other stored procedures.

## Git Branching

The most appropriate branching strategy for a team or project depends on the specific requirements and objectives of the project, the size and structure of the team, and the development methodology being used.

However, there are several well-established branching policies that are commonly used by teams, including:

**Gitflow:** This branching policy involves creating two long-lived branches - a "master" branch for production-ready code, and a "develop" branch for ongoing development. Feature branches are created from the "develop" branch and merged back into "develop" through pull requests.

**Trunk-based Development:** This approach involves only using a single long-lived branch, the "trunk" branch, and committing all changes directly to it. This approach is best suited for small teams with a continuous deployment model.

**Feature Branching:** This involves creating a branch for each feature or change, and merging them back into the main branch once they are complete. This approach works well for small teams and individual developers.

**Release Branching:** This approach involves creating a separate branch for each release, allowing developers to continue working on the next release while the current one is in the testing phase. This approach works well for larger teams and projects with longer release cycles.
