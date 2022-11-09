pipeline {
    agent any
   
    stages {
        stage ('GIT') {
            steps {
                 git branch : 'nour',
                url :'https://github.com/NourelhoudaZemni/DevopsProject.git'
               
            }
           
    }
              stage('MVN CLEAN'){
            steps{
                echo 'Pulling...';
                sh 'mvn clean'
                }
            }
             stage('MVN COMPILE'){
                steps{
                sh 'mvn compile'
                }
             }
             stage('MVN PACKAGE'){
                steps{
                sh 'mvn package'
                }
             }
             stage('MVN TEST'){
                steps{
                 sh 'mvn test'
                 }
             post {
            always {
            junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
        }
        }
             }

        stage('MVN SONAREQUBE') {
              steps {
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=sonar'
            }
           
        }

         
      
   
       stage('nexus deploy') {
        steps{
            sh'mvn deploy '
           
        }
       
       }
           stage('DOCKER BUILD IMG STAGE'){
                steps{
                    script{
                        sh 'docker build -t achat .'
                    }
                   
                }
               
            }
      stage('DOCKER PUSH IMG STAGE '){
                steps{
                    script{
                        withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u root9root -p ${dockerhubpwd}'
                             }
                        sh 'docker tag  achat root9root/achat:latest'    
                        sh 'docker push root9root/achat'    
                    }
                   
                }
               
            }
      stage('DOCKER COMPOSE STAGE'){
                steps{
                    script{
                        sh 'docker-compose up -d'
                    }
                   
                }
               
            }
            
               stage('Email') {
        steps{
            mail bcc: '',
            body: 'Heyy , i am still working ',
            cc: '', from: '',
            replyTo: '', subject: 'The pipeline is working on something here ...',
            to: 'nourelhouda.zemni@esprit.tn'
        }
        }


     
       
    }
}
