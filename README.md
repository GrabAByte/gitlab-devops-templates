# Gitlab CI: Gitlab CI Templates

A central repository containing all CI CD templates to be used in Gitlab across technologies.

## Directory Layout

* `.gitlab-ci.yml` contains the CI pipeline to lint and tag the gitlab CI templates.
* `metadata.json` contains the docker image name and version
* `docs/*.md` contains any additional documentation
* `jobs/ansible/*.yml` contains all ansible related jobs that can be shared across Ansbile repositories.
* `jobs/common/*.yml` contains all common build jobs that can be shared across multiple technologies.
* `jobs/docker/*.yml` contains all docker related jobs that can be shared across all docker repositories.
* `jobs/javascript/*.yml` contains all node JS related jobs that can be shared acroos node JS repositories.
* `jobs/python/*.yml` contains all python related jobs that can be shared across Python repositories.
* `jobs/terraform/*.yml` contains all Terraform relates jobs that can be shared across Terraform repositories.
* `workflows/ansible/*.yml` contain full CI / CD workflows for ansible.
* `workflows/docker/*.yml` contain full CI / CD workflows for Docker.
* `workflows/gitlab/*.yml` contain full CI / CD workflows for Gitlab.
* `workflows/jenkins/*.yml` contain full CI / CD workflows for Jenkins.
* `workflows/javascript/*.yml` contain full CI / CD workflows for Node JS.
* `workflows/python/*.yml` contain full CI / CD workflows for Python.
* `workflows/terraform/*.yml` contain full CI / CD workflows for Terraform.

## Requirements

You will require a gitlab-runner to be provisioned for your project.

Some templates will require that you have particular dependencies installed for each job.

## Dependencies

Notable Depenencies include -

+ aws-cli
+ docker
+ hadolint
+ javascript
+ molecule
+ npm-groovy-lint
+ pylint
+ tflint
+ xml-lint
+ yamllint

## Lint

Gitlab has its own CI Linting Tool however if you wish to be more thorough, run the following command on linux -

```
yamllint .
```

## common
## jobs/common/archive.gitlab-ci.yml
```
Function: This will create a Archive from the calling code base

variables:
  archivePath: '' # default: '', description: The directory and subdirectories to archive
  archiveType: '' # default: '', description: The archive type. Examples: zip, gzip, tar
```
## jobs/common/bashFunctions.gitlab-ci.yml
```
Function: This will run as a pre-script to all jobs when referenced and contains bash functions that will be saved in memory and then can be used.

variables:
  metadataPath: '' # default: '', description: The path to the file containing the project metadata
  nexusCredential: '' # default: "$NEXUS_USER:$NEXUS_PWD" description: The binding username:password credentials for uploading and downloading from Nexus
  nexusUrl: '' # default: '', description: The Nexus URL to use
  nexusGroup: '' # default: '', desctiption: The group namespace to upload or download components
  npmDependencies: '' # default: '', description: Any realms with which to download node JS dependencies
  npmRegistryUrl: '' # default: '', description: The NPM Registry
  npmRegistryDeps: '' # default: '', description: The dependency group
  npmVersion: '' # default: '12.4.0', description: The NPM Version to use
```
## jobs/common/dockerECRLogin.gitlab-ci.yml
```
Function: This will Log into an Elastic Container Registry (for use in AWS only)

variables:
  awsRegion: '' # default: '', description: The AWS region to retrieve credentials from.
  elasticContainerRegistry: '' # default: '', The ECR with which to log in to.
```
## jobs/common/dockerPull.gitlab-ci.yml
```
Function: Pulls a docker image from an docker registry.

variables:
  awsRegion: '' # default: '', description: The AWS region
  elasticContainerRegistry: '' # default: '', description: The name of the container registry with which to pull the docker image.
  imageName: '' # default: '', description: The docker image name to pull from the registry.
```
## jobs/common/gitTag.gitlab-ci.yml
```
Function: This will push a tag to the remote repository. (Only works on your protected branch)

variables:
  gitUser: '' # default: $SVC_USERNAME description: The git user that will push the tags
  gitEmail: '' # default: $SVC_EMAIL description: The git user email
  gitPAT: '' # default: $SVC_ACCESS_TOKEN description: The git token for pushing tags, please ensure this is in your group/project pipeline variables for use
```
## jobs/common/groovyLint.gitlab-ci.yml
```
Function: This will perform a Lint on any *.groovy files in the repository.

variables:
  groovyPath: '' default: '.' description: The relative path which holds the groovy files to be linted
```
## jobs/common/metadata.gitlab-ci.yml (DEPRECATED)
```
Function: This will gather the name and version of the component by reading the metadata.json file in your code repository (metadataPath can be overriden)

variables:
  metadataPath: '' default: '' description: This will read the name and version keys from either a json or yaml file and set those values throughout the jobs in the workflow.
```
## jobs/common/sonarScan.gitlab-ci.yml
```
Function: Scans the project code and published it to Group SonarQube

variables:
  awsRegion: '' # default: '', description: The AWS region
  dockerImage: '' # default: '', description: The docker image which is going to run the Sonar Scan (It must have the SonarQube dependencies installed)
  dockerTag: '' # default: '', decription: The tag of the docker image
  sonarProject: '' # default: "$CI_PROJECT_NAME", description: The name of the project which will be then be displayed in SonarQube
  sonarHostUrl: '' # default: '', description: The group SonarQube URL of where to publish the results
  sonarSources: '' # default: '', description: The directories to publish
  sonarExtraArgs: '' # default: '', description: Any additional arguments to pass to the sonarqube runner command
```
## jobs/common/xmlLint.gitlab-ci.yml
```
Function: Perform a lint on any XML files

variables:
  xmlPath: '' # default: '', description: The path to any XML files with which to check.
```
## docker
## jobs/docker/metadata.gitlab-ci.yml
```
Function: This will gather the metadata for the docker image.

variables:
  imageName: '' default: '' description: The inital name of the docker Image
```
## jobs/docker/dockerLint.gitlab-ci.yml
```
Function: This will pull in the hadolint container and lint your Dockerfile against its rules

variables:
  dockerfilePath: '' default: '' description: The relative path to your Dockerfile to be linted
```
## jobs/docker/dockerBuild.gitlab-ci.yml
```
Function: Builds the docker image locally with the correct namespace and tag

variables:
  dockerfilePath: '' default: '' description: The relative path to your Dockerfile to be linted
  elasticContainerRegistry: '' default: '' description: The name of the ECR to publish the image to
```
## jobs/docker/dockerRepository.gitlab-ci.yml
```
Function: To check if the ECR Repository exists and if it doesnt to create it
```
## jobs/common/dockerECRLogin.gitlab-ci.yml
```
Function: Log into the ECR

variables:
  awsRegion: '' default: '' description: The AWS region in which the ECR resides
  elasticContainerRegistry: ''  default: '' description: The name of the ECR to publish the image to
```
## jobs/docker/dockerTag.gitlab-ci.yml
```
Function: Pull from public registry and tag the docker image locally

variables:
  awsRegion: '' default: '', description: The AWS region in which the ECR resides
  elasticContainerRegistry: '',  default: '' description: The name of the ECR to publish the image to
  imageName: ''  default: '', description: The name of the docker image to pull from public docker registry
```
## jobs/docker/dockerPublish.gitlab-ci.yml
```
Function: To push docker images to the remote ECR for storage
```
## jobs/docker/dockerPrune.gitlab-ci.yml
```
Function: To clear the local docker images from the gitlab-runner

variables:
  cleanup: '' default: false description: Removes any docker images on the gitlab runner that match the project name gathered from the metdata.gitlab-ci.yml job
  elasticContainerRegistry: ''  default: '' description: The name of the ECR to publish the image to
```
## jobs/docker/dockerScan.gitlab-ci.yml
```
Function: Gather the Docker image security scanning results from AWS

variables:
  strictChecking: '' # default: 'true, description: Whether to pass the scan reegardless of Errors.
```
## Node JS
## jobs/javascript/javascriptBuild.gitlab-ci.yml
Function: Build Node JS Based Applications
## jobs/javascript/javascriptFunctions.gitlab-ci.yml (DEPRECATED)
Function: Holds NodeJS based bash functions, replaced by bashFunctions.gitlab-ci.yml
## jobs/javascript/javascriptLint.gitlab-ci.yml
Function: Run Lint on Node JS Based Applications
## jobs/javascript/javascriptStaticTests.gitlab-ci.yml
Function: Run Unit Tests on Node JS Based Applications
## jobs/javascript/javascriptVersion.gitlab-ci.yml
Function: Run versioning scripts on Node JS Based Applications

## Python
## jobs/python/pythonLint.gitlab-ci.yml
```
Function: Performs a lint on any Python code

variables:
  appPath: '' # default: '', description: The relative path to where the application code resides.
  strictChecking: '' default: 'true', description: Whether to allow any linting errors and proceed with the workflow.
```
## jobs/python/pythonStaticTests.gitlab-ci.yml
```
Function: Performs the python unit tests

variables:
  appPath: '' # default: '', description: The relative path to where the application code resides.
  testDirectory: '' # default: '', description: The relative path to where the unit test code resides.
```
## Ansible
## jobs/ansible/ansibleLint.gitlab-ci.yml
```
Function: Perform an Ansible Lint on the code repository

variables:
  ansibleType: '' default: '' description: Whether this is a playbook or role type repository
  strictChecking: '' default: false description: Whether to pass the pipeline regardless of the lint results
```
## jobs/ansible/ansibleMolecule.gitlab-ci.yml
```
Function: (WIP) To perform molecule test on ansible codebase
```
## Terraform
## jobs/terraform/terraformLint.gitlab-ci.yml
```
Function: Lints the Terraform Code with tflint

variables:
  terraformEnvironment: '' default: dev, description: the ./environment/<ENV> subdirectory to change to when running terraform commands.
```
## jobs/terraform/terraformValidate.gitlab-ci.yml
```
Function: Runs the terraform validate

variables:
  terraformEnvironment: '' default: dev, description: The ./environment/<ENV> subdirectory to change to when running terraform commands.
  terraformVersion: '' default: '1.1.0' description: The terraform version to use. (This must be pre-installed on your gitlab runner).
```
## jobs/terraform/terraformTest.gitlab-ci.yml (IN DEVELOPMENT)
## jobs/terraform/terraformPlan.gitlab-ci.yml
```
Function: Runs the terraform plan

variables:
  terraformEnvironment: '' default: dev, description: The ./environment/<ENV> subdirectory to change to when running terraform commands.
  terraformVersion: '' default: '1.1.0' description: The terraform version to use. (This must be pre-installed on your gitlab runner).
```
## jobs/terraform/terraformApply.gitlab-ci.yml
```
Function: Runs the terraform apply (only on "main" branch)

variables:
  terraformEnvironment: '' default: dev, description: The ./environment/<ENV> subdirectory to change to when running terraform commands.
  terraformVersion: '' default: '1.1.0' description: The terraform version to use. (This must be pre-installed on your gitlab runner).
```

# Workflows
## Ansible
## workflows/ansible/ansiblePlaybookCI.gitlab-ci.yml
```
 * jobs/common/metadata.gitlab-ci.yml
 * jobs/common/yamlLint.gitlab-ci.yml
 * jobs/ansible/ansibleLint.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
```
##  workflows/ansible/ansibleRoleCI.gitlab-ci.yml
```
 * jobs/common/metadata.gitlab-ci.yml
 * jobs/common/dockerECRLogin.gitlab-ci.yml
 * jobs/ansible/ansibleMolecule.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
```
## Gitlab
## workflows/gitlab/gitlabCI.gitlab-ci.yml
```
 * jobs/common/metadata.gitlab-ci.yml
 * jobs/common/yamlLint.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
```
## Jenkins
## workflows/jenkins/jenkinsCI.gitlab-ci.yml
```
 * jobs/common/metadata.gitlab-ci.yml
 * jobs/common/groovyLint.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
```
## Docker
## workflows/docker/dockerCI.gitlab-ci.yml
```
 * jobs/common/metadata.gitlab-ci.yml
 * jobs/docker/dockerRepository.gitlab-ci.yml
 * jobs/docker/dockerLint.gitlab-ci.yml
 * jobs/docker/dockerBuild.gitlab-ci.yml
 * jobs/common/dockerECRLogin.gitlab-ci.yml
 * jobs/docker/dockerPublish.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
 * jobs/docker/dockerPrune.gitlab-ci.yml
```
## workflows/docker/dockerUpstreamToInternal.gitlab-ci.yml
```
 * jobs/docker/metadata.gitlab-ci.yml
 * jobs/docker/dockerRepository.gitlab-ci.yml
 * jobs/docker/dockerTag.gitlab-ci.yml
 * jobs/common/dockerECRLogin.gitlab-ci.yml
 * jobs/docker/dockerPublish.gitlab-ci.yml
```
## Python
## workflows/python/pythonCI.gitlab-ci.yml
```
 * jobs/common/bashFunctions.gitlab-ci.yml
 * jobs/common/dockerECRLogin.gitlab-ci.yml
 * jobs/docker/dockerRepository.gitlab-ci.yml
 * jobs/python/pythonLint.gitlab-ci.yml
 * jobs/python/pythonStaticTests.gitlab-ci.yml
 * jobs/common/sonarScan.gitlab-ci.yml
 * jobs/common/archive.gitlab-ci.yml
 * jobs/docker/dockerLint.gitlab-ci.yml
 * jobs/docker/dockerBuild.gitlab-ci.yml
 * jobs/docker/dockerPublish.gitlab-ci.yml
 * jobs/docker/dockerScan.gitlab-ci.yml
 * jobs/common/gitTag.gitlab-ci.yml
```
## Terraform
## workflows/terraform/terraformManagedCICD.gitlab-ci.yml
```
 * 'jobs/common/metadata.gitlab-ci.yml'
 * 'jobs/terraform/terraformLint.gitlab-ci.yml'
 * 'jobs/terraform/terraformValidate.gitlab-ci.yml'
 * 'jobs/terraform/terraformPlan.gitlab-ci.yml'
 * 'jobs/terraform/terraformApply.gitlab-ci.yml'
 * 'jobs/common/gitTag.gitlab-ci.yml'
```
## workflows/terraform/terraformModuleCI.gitlab-ci.yml
```
(WIP)
```

## Author Information
These templates were created in 2021 by GrabAByte, please keep supporting the development community with Open Source Software. 
