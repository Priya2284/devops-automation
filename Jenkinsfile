// =========================================== > s t a r t

pipeline { // pipeline start
    agent any
    tools{
        maven 'maven'
    }
    
    stages{ // stage start 


        // 0
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/aamir490/devops-automation.git']]])
                //git branch: 'main', url: 'https://github.com/aamir490/devops-automation.git'  ( FOR MY UNDERSTADING)
                sh 'mvn clean install'
            }
        }
        
        /*
        NOTE : After '// 0'  go to cmd and  " chmod 777 /var/run/docker.sock " paste this comnd .
         */


        // 1
        stage('Build docker image'){
            steps{
                script{
                    // sh 'docker build -t suresh394/kubernetes .'
                    sh 'docker build -t aamir490/kubernetes .'
                }
            }
        }
        
        
        
        // 2
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'hubpwd', variable: 'mydockerhubpwd')]) {
                    sh 'docker login -u aamir490 -p ${mydockerhubpwd}'
                        
                    }
                    sh 'docker push aamir490/kubernetes'
                }
            }
        }
        

        /*
        NOTE : setup k8s cluster  with 1 mster   &   1 worker node .  
 
        After setuing go to  jenkins  --> manage jenkins --> pluging   -->  advance pluging  ----> chose file 'deploy plugin manaually'  Deploy..
         */


        // 3
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }

    }  // stage close  
} // pipeline close
