# 12-Gitlab

https://docs.gitlab.com/ci/yaml/

GitLab is a web based devops life cycle tool that provides a Git repository manager with features like CI/CD, issue tracking, and project management.

Key Features:

. Version Control: Supports Git for version control, enabling teams to collaborate on code efficiently.

· CI/CD Integration: Automates the build, test, and deployment processes.

. Project Management: Includes tools for issue tracking, time tracking, and project planning.

. Collaboration: Code reviews, merge requests, and wikis to enhance team collaboration.

https://docs.gitlab.com/ci/yaml/

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

##### Artifacts:

<img width="480" alt="image" src="https://github.com/user-attachments/assets/66bfd7d3-28fa-43ac-8781-907e846c9e60" />

<img width="457" alt="image" src="https://github.com/user-attachments/assets/6a89f22f-5a7b-4eba-a0f6-01088be6e88a" />

Gitlab is complete CI/CD tool. It has package repository to store the packages generated during job like jar files, war files, log files, test results etc. All these artifacts can be stored in pakcage manager in gitlab.

And gitlab has container reposiry to store docker images.

And open sources also available, we have Jfrog, Nexus also to store the artifcats.

```
job1:
  script:
    - echo "hello world" > hello.txt
  artifacts:
    name: test-file
    paths:
      - ./hello.txt
      - /bin/
    when: on_success
    access: all
    expires_in: 30 days

```
Artifact (in zip format) is generaed with the name of artifact , here its test-file.zip


##### Variables:
We have use variables key word to use. In below session we see global variables and job based variables

```
# pipeline based variables i.e global variable
variables:  
  name: "suresh"
  company: "ibm"
job1:
  # job based variables, accessable within the job
  variables:  
    location: "chennai"
  script:
    - echo "name and comany are: $name and $company"
    - echo "location is $location "
```

Custom / Project based variables:

Login to git lab -> settings -> CI/CD -> Variables -> Add variable

<img width="533" alt="image" src="https://github.com/user-attachments/assets/c5bb95a1-4204-4f74-acbb-09f9502e98b4" />

<img width="193" alt="image" src="https://github.com/user-attachments/assets/a1f4963c-1b25-49e3-8f73-5c5de8aeb796" />

We can use masked variable to store passwords. Masked/protected variables are not seen in job logs. Its shows as [MASKED]

```
job1:
  script:
    - echo "project is $project"
    - echo "password is $password" 
```
<img width="272" alt="image" src="https://github.com/user-attachments/assets/eeadf0be-3bd1-4e70-89e0-5330222a6957" />

##### built-in / pre-defined variables:
https://docs.gitlab.com/ci/variables/predefined_variables/

```
job1:
  script:
    - echo "project is $CI_PIPELINE_ID"
    - echo "password is $CI_JOB_ID" 
```

TO see all the built-in or pre defined variable values for a job , we can use 'export' command as below
```
job1:
  script:
    - echo "project is $CI_PIPELINE_ID"
    - echo "password is $CI_JOB_ID"
    - export
```
export shows the all the predefined variables and its values.

##### file variable:
Generally when we are using keys (public key, private key etc) for working configuration, we can use these file variables. Also we can store a bigger values in a file and use it.

settings -> variables -> add variable -> choose file type - here give key (its the file variable name) and value.

<img width="529" alt="image" src="https://github.com/user-attachments/assets/bee21235-bb3c-4eaf-b160-6f6d39fd6101" />

```
job1:
  script:
    - echo "file value is $file_var"  # it shows the path of file variable
    - cat $file_var    # it shows the file data
```
<img width="271" alt="image" src="https://github.com/user-attachments/assets/3dbc17d1-14e6-4dfc-a197-6f709a013cca" />

Generally we use, file variable for configuration files, private keys etc

##### dynamic variables / parametaring the variables
<img width="548" alt="image" src="https://github.com/user-attachments/assets/715f49bc-2ff8-4180-ae1d-3cf30d15041e" />

Here the age is 2 printed when we run below pipeline. To pass the value dynamically, click on run pipeline and specify the values above
```
variables:
  age: 2
job1:
  script:
    - echo "age is $age"
```
##### skipping pipe line:
When we commit a pipeline in gitlab it runs automatically. To disable this automatic running of pipeline after commiting, we have to mention commit messages as below

[skip ci]

or

[ci skip]

<img width="170" alt="image" src="https://github.com/user-attachments/assets/91602f6c-10f5-4694-832a-2194838ad2d1" />


<img width="542" alt="image" src="https://github.com/user-attachments/assets/924dbbc7-38da-42e8-b0c3-da5163250ddf" />

##### manual triggering of pipeline

https://docs.gitlab.com/ci/yaml/#when

when keyword can be used to skip the job run after committing the pipeline and we can run the pipeline/job manually

```
job1:
  when: manual
  script:
    - echo "run the job manually"
```

#### rules and only/except:

rules is newly added and only/except will be depricated in upcoming gitlab versions.

<img width="397" alt="image" src="https://github.com/user-attachments/assets/ae741810-f0eb-4205-8aad-ce3599baa433" />

In industry, rules are used when working on feature branch i.e. for testing or dev env, and not deploy to prod.

We can create group project also in gitlab. Groups -> new group -> create group. Then we can create new sub groups or else create new project in the group.

<img width="529" alt="image" src="https://github.com/user-attachments/assets/e41be598-f70d-4480-acf9-42ed385cbdac" />

```
init-job:
  script:
    - echo "its init stage"
build-job:
  script:
    - echo "its build job"
deploy-job:
  script:
    - echo "its deployment job to test"
deploy-prod:
  script:
    - echo "deploy to prod env"
```
Now the above 4 jobs run parallely.

Now create a new branch feature branch from main branch, as below

<img width="566" alt="image" src="https://github.com/user-attachments/assets/47bd36bc-6cc3-4915-ab54-bc5d25c8483b" />

When we create a feature branch, then pipeline running automatically

<img width="555" alt="image" src="https://github.com/user-attachments/assets/487f94d2-b51f-4005-a135-5b22fa3b197c" />

So our requirement is not deploy to prod when its feature branch.

We can choose main / feature branch as below and use only keyword
<img width="587" alt="image" src="https://github.com/user-attachments/assets/5e29bb27-2d23-4d41-abbe-54892b2f2e04" />

```
init-job:
  script:
    - echo "its init stage"
build-job:
  script:
    - echo "its build job"
deploy-job:
  script:
    - echo "its deployment job to test"
deploy-prod:
  only:
    - main  # This job runs only when the code is from main branch. 
  script:
    - echo "deploy to prod env"
```
Usage of rules:
```
init-job:
  script:
    - echo "its init stage"
build-job:
  script:
    - echo "its build job"
deploy-job:
  rules:
    - if: $CI_COMMIT_BRANCH == "main"      # if this condition is true then this job is deployed as we have when condition inside if block as always.
      when: always
    - when: never     # this when condition is like an else part of above if.  if the above condition is not true, then its never deployed.
  script:
    - echo "its deployment job to test"
deploy-prod:
  only:
    - main  # This job runs only when the code is from main branch. 
  script:
    - echo "deploy to prod env"
```
##### workflow:
until now we are placing rules at job level. We can use workflow to place the rules at pipeline level. So the entire pipeline can be controlled.
So workflow is to set the rules at the centralised position.

```
workflow:
  name: test-workflow
  rules:
    - if: $CI_COMMIT_BRANCH == "main"  # this pipeline runs when the commit is for main branch
      when: always
    - when: never    
init-job:
  script:
    - echo "its init stage"
build-job:
  script:
    - echo "its build job"
deploy-job:
  script:
    - echo "its deployment job to test"
deploy-prod:
  script:
    - echo "deploy to prod env"
```

##### understanding merge request
<img width="506" alt="image" src="https://github.com/user-attachments/assets/4c40e1c1-3b4b-43c2-ad77-cc7050d3b12d" />

<img width="460" alt="image" src="https://github.com/user-attachments/assets/bf850b51-a64e-427e-8115-c10380615c7d" />

```
workflow:
  name: test-workflow
  rules:
    - if: $CI_COMMIT_BRANCH == "main" || $CI_PIPELINE_SOURCE == "merge_request_event"     # this pipeline runs when the commit is for main branch or merge request is created
      when: always
    - when: never    
init-job:
  script:
    - echo "its init stage"
build-job:
  script:
    - echo "its build job"
deploy-job:
  script:
    - echo "its deployment job to test"
deploy-prod:
  script:
    - echo "deploy to prod env"
```
Creating merge request in gitlab:

<img width="542" alt="image" src="https://github.com/user-attachments/assets/90b35d93-a4a8-4e86-9d0e-86128097bf25" />

##### images on shared runner:
When we run pipeline in gitlab , pipeline is running on ruby image. We can change it.

```
image: debian:latest    # Global image. all the jobs run on debian, if any job has specified with image, then that job runs on the specified image 
workflow:
  name: test-workflow
  rules:
    - if: $CI_COMMIT_BRANCH == "main" || $CI_PIPELINE_SOURCE == "merge_request_event"     # this pipeline runs when the commit is for main branch or merge request is created
      when: always
    - when: never    
init-job:      # this job runs on debian
  script:
    - echo "its init stage"
build-job:     # this job runs on ubuntu
  image: ubuntu:latest
  script:
    - echo "its build job"
deploy-job:    # this job runs on debian
  script:
    - echo "its deployment job to test"
deploy-prod:    # this job runs on node
  image: node:latest
  script:
    - echo "deploy to prod env"
```

##### installing / downloading packages:
This example is for installing git in ubuntu image and displaying its version
```
image: ubuntu:latest
job1:
  before_script:
    - apt-get update
    - apt-get install git -y    # this installs the git
  script:
    - git version

```
<img width="277" alt="image" src="https://github.com/user-attachments/assets/ee4c2b2a-e9fa-4161-94d5-03c02ebd118d" />

##### loops / matrix:

```
job1:
  script:
    - echo "this is $environment"
  parallel:
    matrix:
      - environment: [dev, test, prod]

#this is dev
#this is test
#this is prod
```
The above one runs are 3 jobs as below

<img width="242" alt="image" src="https://github.com/user-attachments/assets/c90ffbe5-e0a1-4e69-87f5-b97d46cef8a6" />

multi loop / nested loop. Below job repeates 9 times
```
job1:
  script:
    - echo "this is $environment and version is $version"
  parallel:
    matrix:
      - environment: [dev, test, prod]
        version: [1, 2, 3]

#this is dev and version is 1
#this is dev and version is 2
#this is dev and version is 3
#this is test and version is 1
#this is test and version is 2
#this is test and version is 3
#this is prod and version is 1
#this is prod and version is 2
#this is prod and version is 3
```
<img width="226" alt="image" src="https://github.com/user-attachments/assets/941d9a50-f2d4-41bf-892d-a9910652c33c" />



