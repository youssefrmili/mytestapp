// Define global variables before the pipeline
def branchName
def targetBranch

pipeline {
    agent any
    
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: "${scm.branches[0].name}", description: 'Nom de la branche Git')
        string(name: 'CHANGE_ID', defaultValue: "", description: 'ID de changement Git pour les requêtes de fusion')
        string(name: 'CHANGE_TARGET', defaultValue: "", description: 'ID de changement Git pour les requêtes de fusion cibles')
    }
    
    stages {
        stage('Github') {
            steps {
                script {
                    // Initialize variables with parameters
                    branchName = params.BRANCH_NAME
                    targetBranch = params.CHANGE_TARGET

                    git branch: branchName,
                        url: 'https://github.com/youssefrmili/mytestapp.git'

                    echo "Nom de la branche actuelle : ${branchName}"
                    echo "Nom de la branche cible : ${targetBranch}"
                }
            }
        }

        stage('Maven Build') {
            when {
                expression {
                    (params.CHANGE_ID != null) && (targetBranch == 'dev' || targetBranch == 'prod' || targetBranch == 'staging')
                }
            }
            steps {
                sh 'mvn clean install'
                echo 'Étape de construction terminée'
            }
        }
        // Add more stages as necessary...
    }
}
