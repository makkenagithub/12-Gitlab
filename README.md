# 12-Gitlab

GitLab is a web based devops life cycle tool that provides a Git repository manager with features like CI/CD, issue tracking, and project management.

Key Features:

. Version Control: Supports Git for version control, enabling teams to collaborate on code efficiently.

· CI/CD Integration: Automates the build, test, and deployment processes.

. Project Management: Includes tools for issue tracking, time tracking, and project planning.

. Collaboration: Code reviews, merge requests, and wikis to enhance team collaboration.


Adding files to Gitlab repository:
1. we can add files from an ec2 instance by cloning gitlap repos. Then
```
git add .; git commint -m "message"; git push -u origin
```
2. We can add files to gitlap repository using web IDE. Open gitlab project / repos -> Edit -> Web IDE -> (its like VS code) -> add file -> click on source control button and commit.
3. We can add directly from gitlab console. Goto project -> + button -> add file or dorectory -> commit

GitLab Pipeline:

It consists of stages(eg: build, test, deploy) and jobs (compiling code, running tests). Pipeline can be triggered by code pushes, merge requests or scheduled runs.

Pipeline filename syntax:
```
.gitlab-ci.yml
```
Basic pipeline:
```
stages:
  - build
  - test
  - deploy

default:
  image: alpine

build_a:
  stage: build
  script:
    - echo "This job builds something."

build_b:
  stage: build
  script:
    - echo "This job builds something else."

test_a:
  stage: test
  script:
    - echo "This job tests something. It will only run when all jobs in the"
    - echo "build stage are complete."

test_b:
  stage: test
  script:
    - echo "This job tests something else. It will only run when all jobs in the"
    - echo "build stage are complete too. It will start at about the same time as test_a."

deploy_a:
  stage: deploy
  script:
    - echo "This job deploys something. It will only run when all jobs in the"
    - echo "test stage complete."
  environment: production

deploy_b:
  stage: deploy
  script:
    - echo "This job deploys something else. It will only run when all jobs in the"
    - echo "test stage complete. It will start at about the same time as deploy_a."
  environment: production
```







