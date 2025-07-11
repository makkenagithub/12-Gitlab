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

##### Scheduling jobs:
<img width="502" alt="image" src="https://github.com/user-attachments/assets/989198b7-2036-4cef-af9d-4d0e513f740d" />

<img width="207" alt="image" src="https://github.com/user-attachments/assets/bcfa7f3a-d43e-47c6-a056-aa987e722681" />

##### handling job failure:

When the job fails in a stage, then pipeline does not goto next stage. If we want the pipeline to execute next stage even if a job fails, then 
we can add allo_failure: true in that job

```
stages:
  - build
  - test
job1:
  stage: build
  script:
    - echo "build job"
job2:
  stage: build
  script:
    - echooooo "this job fails"
  allow_failure: true
job3:
  stage: test
  script:
    - echo "test job"
```

##### multi-project pipeline / downstream pipeline

<img width="494" alt="image" src="https://github.com/user-attachments/assets/1f99739e-c48b-40ad-8714-f3fa086ff571" />

Another pipeline can be triggered from a pipeline using keyword 'trigger'
create 2 projects in gitlab and a pipeline in each project

project1- pipeline
```
stages:
  - stage1
  - stage2
project1-job1:
  stage: stage1
  script:
    - echo "job1 in project 1"
project1-job2:
  stage: stage2
  trigger:
    project: group2/project2    # <group-name>/<project-name>
    branch: main
```

project2- pipeline:
```
project2-job1:
  script:
    - echo "its project2 downstream job "
```

<img width="319" alt="image" src="https://github.com/user-attachments/assets/de21f88b-9613-4721-8d7b-4e8110d4cc79" />

#### Runners:

<img width="487" alt="image" src="https://github.com/user-attachments/assets/484c44e1-30b9-421a-9db1-a362eb44ab61" />

we use self managed runnrs in realtime

<img width="459" alt="image" src="https://github.com/user-attachments/assets/de151e3f-1ae1-4032-828b-4976596001ca" />

#### Executors:

<img width="498" alt="image" src="https://github.com/user-attachments/assets/88bec4cd-2b23-4076-a9d5-b385aeae6d68" />

<img width="494" alt="image" src="https://github.com/user-attachments/assets/2cb241dd-0f36-4ab2-bf2a-ce7e7f88ddc7" />

<img width="249" alt="image" src="https://github.com/user-attachments/assets/7ea81847-753d-4832-bebe-f0a9dfc35a34" />

##### disabling shared runner:
settings -> ci/cd -> runners -> disable the field 'instance runners for this project'

#### self managed runner:

Create an ec2 in aws using Amazon Linux.

Now come to gitlab home project -> setting -> ci/cd -> runners -> New project runner

<img width="426" alt="image" src="https://github.com/user-attachments/assets/9df1c97c-f562-4d89-8957-50de45ab07b6" />

<img width="419" alt="image" src="https://github.com/user-attachments/assets/5ee0a5a1-5055-4c65-929e-9820145a336b" />

As we did not install anything in EC2, we have install gitlab related things in ec2. Follow the steps shown in gitlab console while registering runner

<img width="180" alt="image" src="https://github.com/user-attachments/assets/da65d0b6-d7eb-4077-a485-c651ea98b43a" />


<img width="421" alt="image" src="https://github.com/user-attachments/assets/e8f81bfd-5502-4e29-98da-ef4a8c6164f8" />

Gitlab version is 17

We can see different executers

<img width="530" alt="image" src="https://github.com/user-attachments/assets/486935c2-5544-4859-8619-e76a2d1c5d4b" />

<img width="422" alt="image" src="https://github.com/user-attachments/assets/30001310-64a4-47b7-b285-27b73ab0eb11" />

<img width="413" alt="image" src="https://github.com/user-attachments/assets/a201bc5b-1dff-4087-ba5e-0b9740d2bb0e" />

We have to add tags in pipeline , so that pipeline chooses the runner with the tags matched

```
job1:
  tags:
    - ec2
    - ohio
    - shell
  script:
    - echo "hello its self manager runner"
```

After commiting the pipeline, its started running/executing in ec2 as below.

Here job s failed because , git is not installed in ec2.

<img width="309" alt="image" src="https://github.com/user-attachments/assets/93691fda-b459-4392-bca5-eb309e0ca461" />

install git in ec2 'sudo dnf intsall git -y' and retry

<img width="316" alt="image" src="https://github.com/user-attachments/assets/a09dc099-9c7c-4d80-b301-2278c792f7eb" />

While creating runners, we have linux, macos, windows as runners platform. For windows, we need to use powershell to execute the commands provided by gitlab.

While registering the runner, we can provide docker as executor and there, we need to give docker image name. FOr eg: assume we have given ubuntu:latest as image. So ubuntu becomes default image. 

We can change this image as below in the pipeline
```
job1:
  image: alpine:latest
  tags:
    - ec2
    - docker
  script:
    - echo "hello, its docker executor"
```

prerequites in ec2 to install docker
```
sudo yum install docker -y
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo systemctl start docker.service
docker ps
sudo chmod 777 /var/run/docker.sock
docker ps
```
#### multiple executors in a single self managed runner:
Create an ec2 in aws.
Now come to gitlab console and create runner (tags are ec2, shell) and follow the steps and run the gitlab-register command in ec2 and give shell as executer. So the runner is created with the self hosted ec2.

Again goto gitlab console and create runner (tags are ec2, docker)  and follow the steps and run gitlab-register command again in the above ec2 and give docker as executer. So 2nd runner is cretaed with the same self hosted ec2.

Then create pipeline as 
```
job1:
  tags:
    - ec2
    - shell
  script:
    - echo "self managed shell executor"
job2:
  tags:
    - ec2
    - docker
  script:
    - echo "self managed docker executor"

```
As a prerequisite we have install few things in ec2 , git for job1 and docker for job2.

https://docs.docker.com/engine/install/linux-postinstall/
```
sudo yum install git -y
sudo yum install docker -y
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo systemctl start docker.service
docker ps
sudo chmod 777 /var/run/docker.sock
docker ps
```

#### Tools setup:
Create a group in gitlab, then create a project in that with read me file. 

java code: 
Clone the repository using git clone https://github.com/DevopsWorking/GitLab-Shell-Java-project-Source-Code.git
sudo mv GitLab-Shell-Java-project-Source-Code web-app

Download the java code from above github link

Then push the java code from ec2 to gitlab project repos.
We have java code and Dockerfile in ec2. How do we push to gitlab repos. Goto ec2 and code path

```
git init
git remote add origin <gitlab repos url>
git add .
git commit -m "add project files"
git remote add origin <gitlab repos url>
git branch -M main
git push -uf origin main
# here it asks for user name and password of gitlab url
```
Now the code is in Gitlab. 

Now set up a runner with shell as executor. Create an ec2 in aws and install basic softwares such as git, docker, and maven.

https://docs.docker.com/engine/install/linux-postinstall/
```
sudo yum install git -y
git -v

sudo yum install docker -y
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo systemctl start docker.service
docker ps
sudo chmod 777 /var/run/docker.sock
docker ps

sudo yum install maven -y
mvn -v
```

Then register the runner. Gitlab consol -> Settings -> ci/cd -> runners -> new project runner with tags as tools, ec2.

WE CAN EVEN RUN THE UNTAGGED JOBS ALSO BY SELECTING THE FIELD HERE
<img width="413" height="210" alt="image" src="https://github.com/user-attachments/assets/38e49a21-1590-4bd4-9231-87b64a95787e" />

Then register the runner in ec2.

MAVEN Commands:
```
mvn -v    # to check the maven version
mvn clean    # is to clear the cache / unnecessary file generated from previous builds
mvn compile    # compile is build command
mvn test    # testing/ generating Junit test cases
mvn package    # generating the jar/war based files.
```
We can directly use the below command to clean and generate package
```
mvn clean package
```

##### maven compile
Now goto the repository where java code is placed and write pipeline .gitlab-ci.yml

```
stages:
  - maven-build
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
```
<img width="281" height="96" alt="image" src="https://github.com/user-attachments/assets/6e835348-16c7-4fc6-a584-546af500f4d3" />

##### Junit test cases:
We have code in ec2. Now to understand, we do manually

In the /src/test/java/com/example/ path 2 Junit test cases are there.

To generate the Junit test reports, run below commands
```
cd <code-path>
mvn test
```
Now the test reports are generated in the path

/target/surefire-reports/


In the src directory we have Junit test cases
<img width="217" height="213" alt="image" src="https://github.com/user-attachments/assets/f6c8efe7-885a-460f-a2b1-0a4bf268a038" />

After running the mvn test command in ec2, test reports are generated in the path as below

<img width="239" height="311" alt="image" src="https://github.com/user-attachments/assets/51325173-7fc9-479c-bf31-ca7e930be117" />

Here two Junit files are generated
<img width="426" height="77" alt="image" src="https://github.com/user-attachments/assets/73eadf13-72ac-4222-b06e-af97c4f738fd" />

Add the test stage in pipeline. After test stage junit artifacts are generated. We have to specify the name of junit files which are going to be generated

https://docs.gitlab.com/ci/testing/unit_test_reports/

https://docs.gitlab.com/ci/testing/unit_test_report_examples/

```
stages:
  - maven-build
  - maven-test
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
maven-test-job:
  stage: maven-test
  tags:
    - tools
    - ec2
  script:
    - mvn test
  artifacts:
    reports:
      junit:
        - target/surefire-reports/TEST-com.example.AppTest-junit.xml
        - target/surefire-reports/TEST-com.example.MyServletTest-junit.xml
    untracked: false
    when: on_success
    access: all
    expire_in: 30 days
```

<img width="283" height="173" alt="image" src="https://github.com/user-attachments/assets/554b1e7b-ee69-45f4-91f1-a4bcad336350" />

Git lab is centralised, we can see the tset reports also as below

<img width="431" height="250" alt="image" src="https://github.com/user-attachments/assets/e6d90b20-29d3-4d07-971a-93108b4e16df" />


##### gitlab package registry
We have jfrog, nexus as package registries in the market.

But gitlab provides the package registry also. we can store maven based war/jar files, nuget(for .net) based packages, npm based packages in gitlab package registry.

<img width="494" height="242" alt="image" src="https://github.com/user-attachments/assets/2e43e65a-4005-4290-adce-6fe41af902eb" />

<img width="493" height="166" alt="image" src="https://github.com/user-attachments/assets/fdde2b57-cc85-4ba9-9371-1b73dcb55708" />

<img width="203" height="236" alt="image" src="https://github.com/user-attachments/assets/82776b4e-0716-4f01-90a7-50bfa0418dff" />

https://docs.gitlab.com/user/packages/package_registry/

```
stages:
  - maven-build
  - maven-test
  - package
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
maven-test-job:
  stage: maven-test
  tags:
    - tools
    - ec2
  script:
    - mvn test
  artifacts:
    reports:
      junit:
        - target/surefire-reports/TEST-com.example.AppTest-junit.xml
        - target/surefire-reports/TEST-com.example.MyServletTest-junit.xml
    untracked: false
    when: on_success
    access: all
    expire_in: 30 days

package-job:
  stage: package
  tags:
    - tools
    - ec2
  script:
    - mvn deploy -s settings.xml

```
In settings.xml and pom.xml we have to provide some info related to gitlab. Its unable to understand completely.

After successful running of above pipeline package is stored in package registry

<img width="559" height="187" alt="image" src="https://github.com/user-attachments/assets/d38c2693-a84a-4e9e-ad88-33ce45ef6a94" />

<img width="552" height="284" alt="image" src="https://github.com/user-attachments/assets/8f4f2e76-9d71-4991-bed1-b3802d43c422" />

This is the alternative of jfrog or nexus package registry

##### building docker image:

```
stages:
  - maven-build
  - maven-test
  - package
  - docker
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
maven-test-job:
  stage: maven-test
  tags:
    - tools
    - ec2
  script:
    - mvn test
  artifacts:
    reports:
      junit:
        - target/surefire-reports/TEST-com.example.AppTest-junit.xml
        - target/surefire-reports/TEST-com.example.MyServletTest-junit.xml
    untracked: false
    when: on_success
    access: all
    expire_in: 30 days

package-job:
  stage: package
  tags:
    - tools
    - ec2
  script:
    - mvn deploy -s settings.xml

docker-build:
  stage: docker
  tags:
    - tools
    - ec2
  script:
    - docker build -t suresh/gitlabtools:$CI_PIPELINE_IID .    # tags should be same as docker hub repository created,  # dynamic versioning with pipeline ID
  after_script:
    - docker images

```
<img width="276" height="83" alt="image" src="https://github.com/user-attachments/assets/7f90f74e-1578-4a3b-8edb-a6989af3372b" />

##### docker commands
<img width="200" height="119" alt="image" src="https://github.com/user-attachments/assets/ecbc78c9-08d1-4884-8feb-4d77efa06de1" />

```
docker login
docker pull [image]
docker push [image]
docker search [term]
```

##### docker push image to docker hub:
create user_id and password variables in gitlab with protected option.

```
stages:
  - maven-build
  - maven-test
  - package
  - docker
  - docker-hub
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
maven-test-job:
  stage: maven-test
  tags:
    - tools
    - ec2
  script:
    - mvn test
  artifacts:
    reports:
      junit:
        - target/surefire-reports/TEST-com.example.AppTest-junit.xml
        - target/surefire-reports/TEST-com.example.MyServletTest-junit.xml
    untracked: false
    when: on_success
    access: all
    expire_in: 30 days

package-job:
  stage: package
  tags:
    - tools
    - ec2
  script:
    - mvn deploy -s settings.xml

docker-build:
  stage: docker
  tags:
    - tools
    - ec2
  script:
    - docker build -t suresh/gitlabtools:$CI_PIPELINE_IID .    # tags should be same as docker hub repository created,  # dynamic versioning with pipeline ID
  after_script:
    - docker images

docker-push:
  stage: docker-hub
  tags:
    - tools
    - ec2
  script:
    - echo "$DOCKER_PASSWORD" | docker login -u $DOCKER_USER_ID --password-stdin
    - docker push suresh/gitlabtools:$CI_PIPELINE_IID      # dynamic versioning with pipeline ID

```
We can use docker login commands as below
```
 - docker login -u $DOCKER_USER_ID -p $DOCKER_PASSWORD
 - echo "$DOCKER_PASSWORD" | docker login -u $DOCKER_USER_ID --password-stdin
```

##### gitlab container registry:

<img width="536" height="266" alt="image" src="https://github.com/user-attachments/assets/4bb30985-5adb-4996-b584-a3c9e7945e0e" />

<img width="301" height="178" alt="image" src="https://github.com/user-attachments/assets/8650787f-16bc-49c6-b39a-c1bfb469042f" />

```
stages:
  - maven-build
  - maven-test
  - package
  - docker
  - docker-container-registry
maven-compile:
  stage: maven-build
  tags:
    - tools
    - ec2
  script:
    - mvn compile
maven-test-job:
  stage: maven-test
  tags:
    - tools
    - ec2
  script:
    - mvn test
  artifacts:
    reports:
      junit:
        - target/surefire-reports/TEST-com.example.AppTest-junit.xml
        - target/surefire-reports/TEST-com.example.MyServletTest-junit.xml
    untracked: false
    when: on_success
    access: all
    expire_in: 30 days

package-job:
  stage: package
  tags:
    - tools
    - ec2
  script:
    - mvn deploy -s settings.xml

docker-build:
  stage: docker
  tags:
    - tools
    - ec2
  script:
    - docker build -t registry.gitlab.com/<project_name>/<image_name>:$CI_PIPELINE_IID .    # tags should be same as docker hub repository created,  # dynamic versioning with pipeline ID
  after_script:
    - docker images

docker-push-gitlab:
  stage: docker-container-registry
  tags:
    - tools
    - ec2
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD registry.gitlab.com
    - docker push registry.gitlab.com/<project_name>/<image_name>:$CI_PIPELINE_IID      # dynamic versioning with pipeline ID

```
After pushing image to gitlab container registry , it looks as below

<img width="535" height="197" alt="image" src="https://github.com/user-attachments/assets/556a1ea1-a7bc-4f8b-a2b2-3f156f999269" />

#### AWS CLI in pipeline:
We need to use access and secret keys to use AWS in gitlab. Create region, access and scret keys in AWS console and store them in Gitlab variables

<img width="253" height="123" alt="image" src="https://github.com/user-attachments/assets/9cc7cdb8-0e80-4db8-b1b8-11fbf04f2214" />

The above variable names already exists in gitlab. Just type AWS and gitlab suggests the name and provide the values.

Also aws configure needs to be done in ec2 where gitlab runner is running. We can add a stage in pipeline and do.

```
stages:
  - maven-build
  - maven-test
  - package
  - docker
  - docker-container-registry
  - aws-configure
aws-job:
  stage: aws-configure
  tags:
    - tools
    - ec2
  before_script:
    - aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
    - aws configure set aws_secret_access_key_is $AWS_SECRET_ACCESS_KEY_ID
    - aws configure set default_region $AWS_DEFAULT_REGION
  script:
    - aws s3 ls    # lists all s3 buckets
```

##### pusing docker image ro aws ECR:
Initially cerate a ECR registry in AWS console and get the docker push commands from ecr

<img width="353" height="269" alt="image" src="https://github.com/user-attachments/assets/3c1a296a-ddc6-4aac-a3fe-ea090795672d" />

<img width="348" height="58" alt="image" src="https://github.com/user-attachments/assets/cdd31aa8-7b0c-4bc6-b127-a2f4c96b9d3d" />

```
stages:
  - maven-build
  - maven-test
  - package
  - docker
  - docker-container-registry
  - aws-configure
  - aws-ecr

package-job:
  stage: package
  tags:
    - tools
    - ec2
  script:
    - mvn deploy -s settings.xml

docker-build:
  stage: docker
  tags:
    - tools
    - ec2
  script:
    - docker build -t <get from aws ecr push commands list>:$CI_PIPELINE_IID .    
  after_script:
    - docker images

aws-configure-job:
  stage: aws-configure
  tags:
    - tools
    - ec2
  before_script:
    - aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
    - aws configure set aws_secret_access_key_is $AWS_SECRET_ACCESS_KEY_ID
    - aws configure set default_region $AWS_DEFAULT_REGION
  script:
    - aws s3 ls    # lists all s3 buckets

aws-ecr-job:
  stage: aws-ecr
  tags:
    - ec2
    - tools
  script:
    - <login command from ecr>
    - <push command from ecr>
```
<img width="413" height="160" alt="image" src="https://github.com/user-attachments/assets/3dfbe0eb-829f-4f87-be48-c9311ad43c3a" />



