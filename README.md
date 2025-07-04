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

https://docs.gitlab.com/ci/pipelines/pipeline_architectures/

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

##### Pipeline editor in GitLab:
We have pipeline editor in gitlab, it gives suggestions, syntax suggestions related info etc. 
<img width="539" alt="image" src="https://github.com/user-attachments/assets/bf696bd8-fd32-4b62-a476-5038c310a5dc" />

<img width="530" alt="image" src="https://github.com/user-attachments/assets/9534359d-5d04-4f17-a465-e8325fd89b0d" />

We have visualisation and validate options also. We can edit the pipe line from pipeline editor and commit it to gilab repos.


Timeout:
```
job1:
  timeout: 10 minutes  # 3 hours 30 minutes or 3h 30m
  script:
    - echo "Hi, this is timeout job example"
```

###### Stages:
<img width="497" alt="image" src="https://github.com/user-attachments/assets/aea0fd02-9af1-4897-a3ab-bbf9e77411fd" />

Multiple stages can be placed in the pipeline. Each stage can have multiple jobs. Stages run sequentially. And jobs in each stage run parallely. 
If a stage fails, then pipeline will be aborted.

```
stages:
  - first
  - second
  - third
job1:
  stage: first
  script:
    - echo "its first stage job"
job2:
  stage: second
  script:
    - echo "its second stahe job"
job3:
  stage: third
  script:
    - echo: "its third stage job"
```

##### Needs:
<img width="527" alt="image" src="https://github.com/user-attachments/assets/06525b34-2e9e-4923-840a-86da2ce90269" />

An array of jobs (maximum of 50 jobs).

An empty array ([]), to set the job to start as soon as the pipeline is created.
```
job1:
  script:
    - echo "hello world"
job2:
  needs:
    - job1
  script:
    - echo "hi- its second job"
  
```
Job2 is dependent on job1 here. If job1 fails then job2 wont run.

Visualisation as below:
<img width="170" alt="image" src="https://github.com/user-attachments/assets/3ab30a1e-f8a5-475a-8e7e-d2d5929cc685" />

##### before_script , after_script:
before_script , after_script are keywords in the job. 

<img width="491" alt="image" src="https://github.com/user-attachments/assets/9241ce62-c749-4656-b1a3-722221f57d51" />

<img width="475" alt="image" src="https://github.com/user-attachments/assets/6ef5eaca-e268-4dc5-b1f4-2272594e19f5" />

```
job1:
  before_script:
    - echo "hi, its before script"
  script:
    - echo "hi, its script"
  after_script:
    - echo "hi its after script"
```
##### linux commands in job
```
job1:
  script:
    - ls -lrt
    - pwd
    - mkdir suresh
```
##### shell script execution:
```
job1:
  script:
    - chmod +x test.sh
    - ./test.sh
```
##### multiple line linux commands:
We can do by using pipe symbol '|' as below or write multiple lines as below
```
job1:
  script:
    - |
        echo "hi"
        ls -lrt
        pwd
        echo "multi line commands using | symbol"

job2:
  script:
    - echo "hi"
    - ls -lrt
    - pwd
    - echo "multi line commands using general way"
```
##### commenting in gitlab pipeline
'#' sybol is used comment single line. In fact multi line comments are not possible in gitlab. But we can comment a job by using '.' dot symbol infront of job , as shown in job2

```
job1:
  script:
    - echo "hi"
    # - ls -lrt
    # - pwd

.job2:
  script:
    - echo "hi"
    - ls -lrt

```








