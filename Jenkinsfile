
pipeline {
    agent any

    stages {
        stage('Check Connection') {
                
            steps {
                script{

                        sshagent(['1e771a24-c0ef-47c4-97ae-ce2f36bd900e']) {
                            sh '''
                            ssh alamin@45.120.115.246 "echo "server running....""
                            '''
               
                        }
              
                    }
                   
                }

            post {
                success {
                       notifyDiscord("Connection success!")
                }
                failure {
                     notifyDiscord("Connection failed!")
                }
            }
            
        }

        stage('update code') {
            steps {
                script{

                       sshagent(['1e771a24-c0ef-47c4-97ae-ce2f36bd900e']) {
                            sh '''
                            ssh alamin@45.120.115.246 "
                            cd /home/alamin/storage/dcl-project/app-test4-deepchainlab-static-frontend  &&
                            sudo git reset .  && 
                            sudo git clean -df &&
                            sudo git stash &&
                            sudo git pull
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

             post {
                success {
                       notifyDiscord("update code  success!")
                }
                failure {
                     notifyDiscord("update code failed!")
                }
            }
       
        }
 

         stage('deploy code') {
            steps {
                script{

                       sshagent(['1e771a24-c0ef-47c4-97ae-ce2f36bd900e']) {
                            sh '''
                            ssh alamin@45.120.115.246 "
                                    cd /home/alamin/storage/dcl-project/app-test4-deepchainlab-static-frontend &&
                                    sudo docker compose up -d --build &&
                                    sudo docker system prune -af
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

              post {
             
                success {
                       notifyDiscord("deploy success!")
                }
                failure {
                     notifyDiscord("deploy failed!")
                }
            }
        }

        
        
    }

}
def notifyDiscord(message) {
    def discordWebhookUrl = 'https://discord.com/api/webhooks/1145653952562597918/b9wR-EbUYiBOOsn3jHGxq1Cg45sGiz45yfo_ASMtAXZzMmhpyvvzQCEl7EL-OBrwUXgS'
    sh "curl -X POST -H 'Content-Type: application/json' -d '{\"content\":\"${message}\"}' ${discordWebhookUrl}"
}
    
