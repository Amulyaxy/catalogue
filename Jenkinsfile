pipeline {
        agent{
            node{
                label 'AGENT-1'
            }
        }
        environment { 
            GREETING = 'Hello Jenkins'
            packageVersion = ''
            nexusURL = '172.31.10.3:8081'
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
                    zip -q -r catlogue.zip ./* -x ".git"
                    ls -ltr
                """
            }
        }
        stage('Publish Artifact') {
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
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
            deleteDir()
        }
        failure {
            echo 'this is run when pipelines failed, used generallt to send some alerts'
        }
        success {
            echo 'I will say hello pipeline is success'
        }
    }
}
    

