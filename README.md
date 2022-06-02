<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)
[![discuss.overhang.io](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=ff0080&message=discuss.overhang.io)](https://discuss.overhang.io)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-enable-backup

Github Action to install and enable the Tutor plugin - Open edX Discovery service



## Usage:


```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      ENABLE_DISCOVERY: true

    steps:
      # required antecedent
      - uses: actions/checkout@v3.0.2

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1.6.1
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

    - name: Install and configure kubectl
      run: |-
        sudo snap install kubectl --channel=1.23/stable --classic
        aws eks --region us-east-2 update-kubeconfig --name mrionline-global-live --alias eks-prod

    # install Tutor which we'll use for configuring and deploying Open edX
    - name: Install Tutor
      run: |-
        sudo apt install python3 python3-pip libyaml-dev
        pip install --upgrade pyyaml
        echo "TUTOR_ROOT=$GITHUB_WORKSPACE/tutor" >> $GITHUB_ENV
        pip install tutor
      shell: bash

    # This action.
    - name: Enable tutor plugin - Discovery service
      uses: openedx-actions/tutor-enable-plugin-discovery@v0.0.1
      if: ${{ env.ENABLE_DISCOVERY == 'true' }}
      with:
        namespace: openedx-prod
```
