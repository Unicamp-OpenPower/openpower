---
title: Using GitLab CI with GitHub repositories
layout: page
date: 2022-06-19
authors: [guilhermedobins]

aliases: [/blog/gitlab-ci-with-github-repo.html]

---

Despite GitHub offering some options for CI-CD platforms, such as Travis and Actions, you may want to try out GitLab’s alternatives, but feel turned off by the fact you might need to take your codes to a different platform. To avoid this problem, using these two platforms together sounds like a good idea. The goal of this tutorial is to show you how to configure a GitLab runner, a pipeline, and use the GitLab-CI to run your GitHub pipelines, without the need for premium services.

For this tutorial, you will need to be registered to GitHub and GitLab, and also a computer where the GitLab-runner will be installed.

## Configuring the GitLab repo
The first thing you need to do is to create a repository on GitLab. In this repository, the only thing you have to do is to create a file called ".gitlab-ci.yml". This file works as a "travis.yml" file and is responsible for the configuration of the pipeline, such as the steps to be executed, installing the prerequisites... You can find more information about it in the [gitlab documentation](https://docs.gitlab.com/ee/ci/yaml/gitlab_ci_yaml.html). For this tutorial, the goal is to use this file to retrieve the contents of the GitHub repository. The easiest way of doing this is to clone the repository before running the script. 

To make it clear, suppose your GitHub repository contains a "build.sh" file, and your pipeline consists of running this script. Then, your .gitlab-ci.yml would look something like this:
```
default:
  tags:
    - *your_runner_tag* #This tag is used to select a runner

stages:
  - build

before_script:
  - git clone *your_github_repo_url*

build:
  stage: build
  script: 
    - bash github_repo/build.sh
```

## Setting up a GitLab-runner
The next part of the configuration is to set up a runner. To do so,  you will need a computer where the runner will be installed. One suggestion is to use a [minicloud VM](https://openpower.ic.unicamp.br/minicloud/) for this purpose. 
After connecting to the VM, the next step is to install the runner. To do that, you can follow the steps described below. For more details, you can check the [gitlab documentation](https://docs.gitlab.com/runner/install/linux-manually.html#using-binary-file).
First, download the runner
```bash
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-ppc64le"

sudo chmod +x /usr/local/bin/gitlab-runner
```


Then, you must create a user for the runner. 
```bash
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

Note: Make sure to give this user the right permissions to execute your pipeline. For example, make sure this user is in the sudoers list if you plan to execute any sudo commands. Also, if you want the pipelines to be run as an already existing user, you can skip the previous step.

Finally, install and execute the runner
```bash
sudo gitlab-runner install --user=gitlab-runner –working-directory=/home/gitlab-runner

sudo gitlab-runner start
```

Notice that, sometimes, due to an error, the working directory might not be set to the option you wanted. To fix this, you must edit the /etc/systemd/system/gitlab-runner.service file, and change the working directory to the right one. You might need to reboot your computer to apply this change.

Now that the GitLab runner is installed, you must register it, so it will be connected to your GitLab account.

```bash
sudo gitlab-runner register
```

After that, type in the GitLab instance URL you are using (For example, https://gitlab.com/)
Then, your registration token will be required. To find it, go to the GitLab website, enter the repo you created for this CI project, and then go to “Settings” > “CI/CD”.  After that, click “Expand” on the right side of the “Runners” section. In the “Specific Runners” tab, you will find your registration token, copy it, and paste it to your terminal to continue the runner registration. 

The next step is to name your runner, so you can identify it later. Finally, you must add tags to the runner.  These tags will make it easier for you to choose a runner for your pipelines. For example, you can install python on this runner, then add the "python" tag to it, so you can use it on pipelines where python is required.

Then, you will be asked to enter a maintenance note, but you can just hit "enter" and leave it blank. 

For the final step on the runner setup, you need to choose an executor. The main ones are docker and shell. If you choose docker, you must provide a docker image on the pipeline configuration, and your pipeline will be run on a container build with said image. On the other hand, if you choose "shell", the pipeline will be executed on the shell of the computer where the runner was installed. Notice that, in order to use the docker executor, docker must be previously installed. After this choice, the runner is completely installed.

Before you continue with the configuration, make sure to remove the ".bash_logout" file from the gitlab-runner user's home folder, because if you don't, an error will occur during the pipeline execution. Also, back on the GitLab website, in the Runners section you just visited, make sure to disable the "Allow shared runners for this project" option, under "Shared Runners” so your own runners will always be used.


## Linking the GitHub and GitLab repositories
The last thing you have to do is to link both repositories so that whenever a change is made to GitHub, it will trigger the GitLab pipeline. 
First, go to the GitLab repository, then "Settings" > "CI/CD". Click the "Expand" button next to "Pipeline triggers" and create a new trigger by giving it a description and pressing "Add trigger". After that, find the "Use webhook" option under the tokens and copy this URL, but make sure to change the REF_NAME to be the branch you are using for the project (possibly the main branch), and also change the TOKEN variable to the value of the token you just created. 

With this URL, head to your GitHub repository, and click "Settings" > "Webhooks" (under the "Code and automation" section). Click "Add webhook", and under "Payload URL", add the URL you got from GitLab (with the correct values for REF_NAME and TOKEN). You can leave the other options as they are and then add the webhook.

After all those steps, every time you make a change to your GitHub repository, it should trigger GitLab, and use the runner you just configured to execute a pipeline.

