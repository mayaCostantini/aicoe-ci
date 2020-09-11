# AICoE-CI

AICoE-CI is a continuous integration and delivery system based Tekton-Pipeline/OpenShift-Pipeline.

AICoE-CI is developed with Tekton Pipeline concepts.Pipeline are triggered with Tekton Triggers which functions based on git events.<br>
Aicoe-ci provides GitHub automation, serves on webhook events based on Pull Request, Issues and Tag releases.<br>
On the webhook triggered events, different pipeline are triggered to provide different services.

# Getting Started

## Setting AICoE-CI on GitHub Organization/Repository

Setting up AICoE-CI on to your Github Organization/Repository can be commenced with three simple steps process:

- AICoE-CI is available as [github application](https://github.com/apps/aicoe-ci).<br>
  Install and configure the app on your GitHub Organization or Repository.

- Send invite to our friendly cyborg bot as a Contributor: [Sesheta](https://github.com/sesheta)

- Add the aicoe-ci configuration file into your repository/repositories.<br>
  Example configuration file: [.aicoe-ci.yaml](docs/.aicoe-ci.yaml)<br>
  More Detailed Explanation on aicoe-ci configuration file: [aicoe-ci configuration file](#aicoe-ci-configuration-file)

**NOTE**: Interested in Other Thoth-bots, AICoE-CI works very well with other bots. More information on Setup of other Thoth Bots: [Thoth Bots Setup](docs/thoth-bots-setup.md)

## AICoE-CI configuration file

The AICoE-CI configuration file is to be added to the root of the repository directory and to be named: `.aicoe-ci.yaml`<br>
It allows the user to configure checks, release modules to pypi, build preferences and update imagestream in kustomize yaml file.

### Configuring checks and tests

CI provides following checks for pull requests.Each check is independent of each other, so users can pick and choose the tests which they require for there repository. It can be added into the `.aicoe.yaml` configuration file, as shown in the example snippet below.

```yaml
check:
  - thoth-pytest
  - thoth-precommit
  - thoth-build
  - thoth-custom-build
```

More information on available checks/tests:

- `thoth-pytest`: executes python-3.6 pytest.
- `thoth-precommit`: execute [pre-commit](https://pre-commit.com/) checks based on [pre-commit-config.yaml](.pre-commit-config.yaml).
- `thoth-build`: execute container image build check. more information on configuring image build requirements: [here](#configuring-build-requirements).
- `thoth-custom-build`: execute custom pipeline available in the users repository.

If users require more custom checks/tests, please open a feature request issue with detailed explanation of the tests requirement.

### Configuring Build requirements

Configuration files allows user assign details about the build requirements and specify base image and registry details for build and push.

## Services and Features

- Status Check on Pull request for testing and containerization.

  - `aicoe-ci/pre-commit-check` execute [pre-commit](https://pre-commit.com/) checks based on [pre-commit-config.yaml](.pre-commit-config.yaml).
  - `aicoe-ci/pytest-check` executes run python test.
  - `aicoe-ci/coala-check` executes [coala](https://coala.io/#/home) linting checks based on [.coafile]() (**Deprecated**)
  - `aicoe-ci/build-check` execute container image build check.<br>
    Supports source-to-image, Dockerfile, Containerfile based image. (Default `source-to-image`, if Dockerfile or Containerfile are not present)

- Building Container image on Tag release and for Pull request.

  - On Tag release, the pipeline would build the image based upon the tag and release to [quay.io](https://quay.io/).
  - Pull request based image can be build by commenting `/deploy` on the pull-request comment to release the image to [quay.io](https://quay.io/).

### GitHub ChatOps Options:

AICoE-CI provides some GitHub comment commands for pull request processing on GitHub.<br>
Developers can comment following commands on Github pull request.

- `/retest`
- `/deploy`

All the Pull Request status checks can be rerun by the following command.<br>
command: `/retest`

In a Development environment, if the application is setup with image registry in the aicoe-ci configuration file. Then with the following command, aicoe-ci can assist with building an image out of the pull request and pushing the image to configured image registry.<br>
command: `/deploy`

# How to Contribute

- For Overview on AICoE-CI Pipeline, check: [AICoE-CI Development]()
- For Contribution Details and Setup of dev environment: [Setup and Contribution](docs/how-to-contribute.md)
