<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)
[![discuss.overhang.io](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=ff0080&message=discuss.overhang.io)](https://discuss.overhang.io)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-enable-discovery

Github Action to install and enable the Tutor plugin - Open edX Discovery service

This action is designed to work seamlessly with Kubernetes secrets created by the Terraform modules contained in [Cookiecutter Tutor Open edX Production Devops Tools](https://github.com/lpm0073/cookiecutter-openedx-devops).

**IMPORTANT SECURITY DISCLAIMER**: Sensitive data contained in Kubernetes secrets is masked in Github Actions logs and console output provided that the secret was created with the Terraform scripts provided in the Cookiecutter. If you are working a Kubernetes secret created outside of the Cookiecutter then **be aware that you run a non-zero risk of your sensitive data becoming exposed inside the Github Actions log data and/or console output**.

## Usage:


```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # required antecedent
      - uses: actions/checkout@v3.5.0

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor and kubectl
      - name: Configure Github workflow environment
        uses: openedx-actions/tutor-k8s-init@v1.0.8

      #
      # ... steps to deploy your Open edX instance to k8s ...
      #

      # This action.
      # - tutor-discovery-version: optional. defaults to "latest"
      - name: Enable tutor plugin - Discovery service
        uses: openedx-actions/tutor-enable-plugin-discovery@v1.0.1
        with:
          namespace: openedx-prod
          tutor-discovery-version: "latest"

      #
      # ... more steps to deploy your Open edX instance to k8s ...
      #

```
