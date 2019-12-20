pipeline {
    agent none
    stages {
        
        stage ('Non-Parallel Stage') {
            agent {
                label ('master')
            }
            steps {
                echo "Checking out from Git Repo";
                git credentialsId: '58c60d70-182f-4a7e-afc8-1def90df164e', url: 'https://github.com/Ramzan-786/Testing.git'
            }
        }
        
        stage ('Run Tests') {
            parallel {
                
                stage ('Build: Test on Slave Machine') {
                    agent {
                        label ('WindowsMachine')
                    }
                    steps {
                        echo "Building the checked-out project";
                        git credentialsId: '58c60d70-182f-4a7e-afc8-1def90df164e', url: 'https://github.com/Ramzan-786/Testing.git'
                        bat label: '', script: 'Build.bat'
                    }
                }
                
                stage ('JUnit: Test on Master Machine') {
                    agent {
                        label ('master')
                    }
                    steps {
                         echo "Running JUnit Tests";
                         bat label: '', script: 'Test.bat'
                    }
                }
                
                stage ('Quality-Gate:Test on Slave Machine') {
                    agent {
                         label ('WindowsMachine')
                    }
                    steps {
                        echo "Varifying Quality Gates";
                        bat label: '', script: 'Quality.bat'
                    }
                }
                
                stage ('Deploy: Test on Slave Machine') {
                    agent {
                        label ('master')
                    }
                    steps {
                        echo "Deploying stage envioroment for more tests";
                        bat label: '', script: 'Deploy.bat'
                    }
                }
        
            }
        }
    }
}
