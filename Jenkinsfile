// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            yaml '''
kind: Pod
metadata:
  name: kaniko
  namespace: default
spec:
  containers:
  - name: shell
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: IfNotPresent
    env:
     - name: container
       value: "docker"
    command:
     - /busybox/cat
    tty: true
    volumeMounts:
      - name: docker-config
        mountPath: /kaniko/.docker
  volumes:
    - name: docker-config
      configMap:
        name: docker-config
'''
            defaultContainer 'shell'
        }
    }
    stages {
       stage('Build') {
           steps {
             container('shell'){
               sh "/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=${env.ECR_REPO}:${env.BUILD_ID}"
          }
        }
        }
}
}
