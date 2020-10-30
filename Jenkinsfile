pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: openjdk
    image: openjdk
    command:
    - cat
    tty: true
  - name: docker
    image: docker
    command:
    - cat
    tty: true

"""
    }
  }
      stages {
        stage('Example Build') {
         steps {
        container('maven') {
          sh 'mvn -version'
          sh 'mvn clean package'

        }
         container('docker') {
          sh 'docker --version'
          sh """
              sudo groupadd docker
              sudo usermod -aG docker $USER
              chmod 777 /var/run/docker.sock
          """
          sh 'docker build -t cris/petclinic .'
          sh 'snap logs docker'
          // sh 'docker run -p 8080:8080 --user root -v /var/run/docker.sock:/var/run/docker.sock cris/petclinic'
        }

      }
        }
   /*     stage('Example Test') {
           
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
               
            }
      
        }*/ 
}

}
