pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Clone'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'git', url: 'https://github.com/nagasaisameeta/task-10.git'
                }
            }
        }
    stage('Maven Build'){
        
        steps{
            
            script{
                def mavenHome = tool name: "Maven-3.9.1", type: "maven"
                def mavenCMD = "${mavenHome}/bin/mvn"
                sh "${mavenCMD} clean package"
            }
        }
      }
      stage('SonarQube analysis'){
          steps{
              script{
                 withSonarQubeEnv(credentialsId: 'sonar') {
                   def mavenHome = tool name: "Maven-3.9.1", type: "maven"
                   def mavenCMD = "${mavenHome}/bin/mvn"
                   sh "${mavenCMD} sonar:sonar"
                 }
              }
          }
      }
    
       stage('upload war to nexus'){ 
           
           steps{
                script{
                    
                   nexusArtifactUploader artifacts: [
                       [
                           artifactId: 'sai-1', 
                           classifier: '', 
                           file: 'target/sai-1-3.9.8.jar', 
                           type: 'jar'
                           ]
                           ], 
                           credentialsId: 'nexus', 
                           groupId: 'sai', 
                           nexusUrl: '34.228.42.223:8081/', 
                           nexusVersion: 'nexus3', 
                           protocol: 'http', 
                           repository: 'sai-release', 
                           version: '3.9.8'
               } 
           }
        }
        
    }
}
