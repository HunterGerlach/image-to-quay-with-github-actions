# image-to-quay-with-github-actions

This is an example repository to show/describe how to build a container image and push to Quay.io using GitHub Actions.

## Code Repository

### Container Definition
This repository is setup to build an image (`jenkins-agent-maven-python`) when a PR is made to any contents in the relevant image path (as defined in the GitHub Actions workflow file). 

### GitHub Actions Workflow Definition

The `.github/workflows` directory contains the GitHub actions yaml definition files. These are automatically recognized by GitHub upon commit. Each of the definitions has the following areas that are necessary to configure a working action.

- `branches`: This is a list of branches that monitored for changes to identify when an action should be started.
-  `paths`: The path to monitor for changes. Can be a single file or a wildcard.
- `env -> image_name`: This is the name that is configured in the quay.io regisitry. 
- `registry`: The pre-defined registry used for image publication. See GitHub Secrets
- `repository`: The pre-defined image repository. See GitHub Secrets
- `username`: For use in accessing the remote registry. See GitHub Secrets
- `password`: For use in accessing the remote registry. See GitHub Secrets

## GitHub Secrets

In the repository settings, add the following secrets:

 -`REGISTRY_URI`: `quay.io`
 -`REGISTRY_REPOSITORY`:  _Your quay.io username or organization name_
 -`REGISTRY_USERNAME`:  _Your quay.io username or organization name (this is likely the same as the repository name)_
 -`REGISTRY_PASSWORD`:  _Your quay.io password_

## Image Repository Settings on Quay.io

In Quay.io, create a new repository that has the same value as the `env -> image_name` listed in your GitHub Action yaml file. 

**Note:** Keep in mind that this value will likely be different for actions that create different container images, but will likely be the same for actions which are performing different steps on the same container (e.g. PR vs Publish).

## GitHub Actions

Assuming that everything is configured correctly, you should now be able to see GitHub Actions in action for this repository.

`jenkins-agent-maven-python-pr` will begin when any file in the `jenkins-slaves/jenkins-slave-maven-python/` path is modified and committed. This can be on any branch.

`jenkins-agent-maven-python-publish` will begin when the `version.json` file is updated in the `jenkins-slaves/jenkins-slave-maven-python/` path. This will likely only occur during a major/minor version release which is tagged and pushed manually.
