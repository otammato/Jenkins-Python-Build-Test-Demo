# jenkins-python-build-test-demo
<br><br>
#### Installation with Docker and installing python3 and pytest inside the Docker container.
- Install Docker on your local machine.
- Run this command: docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
- Write down the password that's created for you during this first time set up process: like 8458e513eacd41d8875ca3253f47d3a0
- While the docker container is running, run cmd: docker ps to see what containers are running - copy the container ID for Jenkins, like 8f7c957e19fd
- Run command: docker exec -it -u 0 8f7c957e19fd /bin/bash to open an interactive terminal within the Docker Container as root (user 0)
- Run command: apt-get update and apt-get install python3 and apt-get install python3-pip to install Python3 and pip within the Docker container
- Run pip install pytest to install the pytest package that actually runs the unit/integ tests during your test stage within the pipeline
<br><br>
#### Jenkins log in
- Go to localhost:8080 and you should be prompted for the password from a previous step
- Log in to Jenkins<br><br>
#### Create 3 stages: "Hello", "Build" and "Test"
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

#### Test if a pipeline does work as expected

<br><br>
<p align="center">
  <img src="images/Screenshot 2023-01-11 at 14.17.20.png" width="700"/>
</p>
<br><br>
