pipeline{
    agent none
     options {
    buildDiscarder(logRotator(numToKeepStr: '3'))
  }
    stages{
        stage("build name"){
            agent{label"linux-node"}
            steps{
                buildName 'Yuga'
            }
        }
        
        stage("Download"){
            agent{label"master"}
            steps{
                            git credentialsId: 'bc9b3ce7-a257-46ae-9d40-824f939c81d4', url: 'https://github.com/Yuga-23/caramel.git'
                        
            }
        }
        stage("build"){
            agent{label"linux-node"}
            steps{
                sh 'mvn package'
                
            }
        }
        stage("archive"){
             agent{label"master"}
            steps{
                // This step should not normally be used in your script. Consult the inline help for details.
archive '**/*.war'
                
            }
        }
        stage("Deploy"){
             agent{label"linux-node"}
            steps{
                sh 'scp /home/ubuntu/workspace/yugandhar_pipeline/target/app.war  ubuntu@172.31.19.219:/home/ubuntu/appserver/yuga.war'
               
            }
        }
        stage("Nexus deploy"){
            agent{label"linux-node"}
            steps{
               nexusPublisher nexusInstanceId: 'nexus_server', nexusRepositoryId: 'yuga-repo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/home/ubuntu/workspace/yugandhar_pipeline/target/app.war']], mavenCoordinate: [artifactId: 'app', groupId: 'caramelit', packaging: 'war', version: '3.0']]]
            }
        }
        stage("Email"){
            steps{
                mail bcc: '', body: 'Hai yugandhar please check the job', cc: '', from: '', replyTo: '', subject: 'Every build', to: 'yugandhar.prathi@gmail.com'
                
            }
        }
    }
}
