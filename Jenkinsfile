pipeline {
        agent{
            node{
                label 'AGENT-1'
            }
        }
        environment { 
            GREETING = 'Hello Jenkins'
            packageVersion = ''
        }
        options {
            timeout(time: 1, unit: 'HOURS')
            disableConcurrentBuilds()
        }
    //     parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
    stages{
        stage('Get the version') {
            steps {
               script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
               }
            }
        }
        stage('Install dependencies'){
            steps {
                sh """
                    npm install
                """
            }
        }
   
        stage('Build') {
            steps {
                sh """
                    ls -la
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    echo   "Here I wrote shell script"
                    echo "$GREETING"
                    #sleep 10
                """
            }
        }
      
    }
    post { 
        always {
            echo 'I will alwasy say hello!'
        }
        failure {
            echo 'this is run when pipelines failed, used generallt to send some alerts'
        }
        success {
            echo 'I will say hello pipeline is success'
        }
    }
}
    

