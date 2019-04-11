pipeline {

    agent any

    stages {

        stage('Propagate') {
            parallel {
                stage('STAGING') {
                    agent { 
                        docker { 
                            image 'pmaugeri/alpine-ak-cli' 
                            args '--mount source=stockholm-devops,target=/root/data -u 0:0'
                        }
                    }
                    steps {
                        echo "Updating STAGING environment ..."
                        sh 'akamai property update stockholm_STAGING --srcprop stockholm_master'
                    }
                }
            }
        }


        stage('Activate STAGING') {
            parallel {
                stage('STAGING/Staging') {
                    agent { 
                        docker { 
                            image 'pmaugeri/alpine-ak-cli' 
                            args '--mount source=stockholm-devops,target=/root/data -u 0:0'
                        }
                    }
                    steps {
                        echo "Activate STAGING environment on Staging..."
                        sh 'akamai property activate stockholm_STAGING --network staging'
                    }
                }
                stage('STAGING/Production') {
                    agent { 
                        docker { 
                            image 'pmaugeri/alpine-ak-cli' 
                            args '--mount source=stockholm-devops,target=/root/data -u 0:0'
                        }
                    }
                    steps {
                        echo "Activate STAGING environment on Production..."
                        sh 'akamai property activate stockholm_STAGING --network prod'
                    }
                }
            }
        }


   }

}
