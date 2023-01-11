# CI/CD jenkins-python-build-test-demo
<br><br>
#### 1. Jenkins installation with Docker and installing python3 and pytest inside the Docker container.
- Install Docker on your local machine.
- Run this command: docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
- Write down the password that's created for you during this first time set up process: like 8458e513eacd41d8875ca3253f47d3a0
- While the docker container is running, run cmd: docker ps to see what containers are running - copy the container ID for Jenkins, like 8f7c957e19fd
- Run command: docker exec -it -u 0 8f7c957e19fd /bin/bash to open an interactive terminal within the Docker Container as root (user 0)
- Run command: apt-get update and apt-get install python3 and apt-get install python3-pip to install Python3 and pip within the Docker container
- Run pip install pytest to install the pytest package that actually runs the unit/integ tests during your test stage within the pipeline
<br><br>
#### 2. Jenkins log in
- Go to localhost:8080 and you should be prompted for the password from a previous step
- Log in to Jenkins<br><br>
<br><br>

#### 3. A code to be deployed and test cases.
#### 3.1. The ops.py script:
This is a Python script that defines four functions: "add," "subtract," "multiply," and "divide".

The "add" function takes in two arguments, "x" and "y," and returns the sum of the two arguments.

The "subtract" function takes in two arguments, "x" and "y," and returns the difference of the two arguments (x minus y).

The "multiply" function takes in two arguments, "x" and "y," and returns the product of the two arguments.

The "divide" function takes in two arguments, "x" and "y," and returns the quotient of the two arguments (x divided by y).
<pre>
def add(x,y):
    return x+y

def subtract(x,y):
    return x-y

def multiply(x,y):
    return x*y

def divide(x,y):
    return x/y
</pre>

#### 3.2. The test_ops.py script with four test cases:

<pre>
</pre>

#### 3. Create 3 stages: "Hello", "Build" and "Test"
<br>
This following script is a Jenkins pipeline written in the Jenkins Pipeline Domain Specific Language (DSL). The pipeline is comprised of three stages: "Hello," "Build," and "Test."

In the "Hello" stage, the pipeline uses the "checkout" step to check out the code from a Git repository, specifically the 'main' branch of a repository at the URL "https://github.com/otammato/jenkins-python-build-test-demo.git".

The "Build" stage uses the "git" step to again specify the repository and branch to check out (main branch of the same repository) and "sh" step 'python3 ops.py' to run some python command. In Jenkins, the "sh" step is usedto run shell commands as part of the build process. The step takes a single argument, which is the command to be executed.

In the "Test" stage, the pipeline uses the "sh" step to run the command "python3 -m pytest", which runs the pytest test runner on the code that was checked out in the previous stages. This will run the test cases on the code checked out.

Overall, this pipeline is checking out code from a Git repository, running some python script and then running the test cases on the code using Pytest.
<br>
<pre>
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/otammato/jenkins-python-build-test-demo.git']])
            }
        }
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/otammato/jenkins-python-build-test-demo.git'
                sh 'python3 ops.py'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }
    }
}
</pre>


<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 12.36.34.png" width="700"/>
</p>
<br><br>

#### 4. Test if a pipeline does work as expected

<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 14.29.42.png" width="700"/>
</p>
<br><br>

<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 14.24.06.png" width="700"/>
</p>
<br><br>

The green indicates all stages completed sucessfully and log shows that all test cases are passed sucessfully. This means that our code from ops.py works correctly and can be deployed to a production environment.

<br><br><br><br>
#### 5. Now we intentionally compromise one test case and check the log

For the first test case we check if 2+3 equals 6

<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 14.46.02.png" width="700"/>
</p>
<br><br>

Then launch a build again. Now it shows red.
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 14.59.28.png" width="700"/>
</p>
<br><br>

Check the log
<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 15.18.38.png" width="700"/>
</p>
<br><br>
The log shows now that 1 of 4 test is failed and explains why
<br><br>
<br><br>

This overall results show that our pipeline works as expected
