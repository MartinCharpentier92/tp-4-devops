pipeline {
    agent any

    environment {
        CONTAINER_ID = ''
        SUM_PY_PATH = 'C:/Users/charp/desktop/tp-4-devops/sum.py'
        DIR_PATH = 'C:/Users/teo/desktop'
        TEST_FILE_PATH = 'C:/Users/charp/Desktop/tp-4-devops/test_variables.txt'
    }

    stages {
        stage('Initialisation') {
            steps {
                echo "Agent OUI"
                echo "Fichier sum.py : ${env.SUM_PY_PATH}"
                echo "Dockerfile dans : ${env.DIR_PATH}"
                echo "Fichier de test : ${env.TEST_FILE_PATH}"
            }
        }

        stage('Build') {
            steps {
                echo "Construction de l'image docker"
                sh "docker build -t container-tp-4-devops-image ${env.DIR_PATH}"
            }
        }

        stage('Run') {
            steps {
                script {
