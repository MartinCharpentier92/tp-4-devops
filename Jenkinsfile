pipeline {
    agent any

    environment {
        SUM_PY_PATH = 'C:/Users/charp/desktop/tp-4-devops/sum.py'
        DIR_PATH = '.'
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
                    echo "Lancement du conteneur Docker..."
                    def output = sh(script: "docker run -dit container-tp-4-devops-image", returnStdout: true).trim()
                    // On écrit dans un fichier pour le récupérer plus tard
                    writeFile file: 'container_id.txt', text: output
                    echo "Conteneur lancé avec ID : ${output}"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // On récupère l'ID stocké
                    def containerId = readFile('container_id.txt').trim()

                    def testLines = readFile(env.TEST_FILE_PATH).split('\n')

                    for (line in testLines) {
                        if (line.trim() == '') continue

                        def vars = line.trim().split(' ')
                        def arg1 = vars[0]
                        def arg2 = vars[1]
                        def expectedSum = vars[2].toFloat()

                        def output = sh(
                            script: "docker exec ${containerId} python /app/sum.py ${arg1} ${arg2}",
                            returnStdout: true
                        )

                        def result = output.split('\n')[-1].trim().toFloat()

                        if (result == expectedSum) {
                            echo "✅ Test réussi : ${arg1} + ${arg2} = ${result}"
                        } else {
                            error "❌ Test échoué : ${arg1} + ${arg2} = ${result}, attendu : ${expectedSum}"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo "🧹 Nettoyage du conteneur Docker"
            script {
                def containerId = fileExists('container_id.txt') ? readFile('container_id.txt').trim() : ''
                if (containerId) {
                    sh "docker stop ${containerId} || true"
                    sh "docker rm ${containerId} || true"
                } else {
                    echo "Aucun ID de conteneur trouvé pour le nettoyage."
                }
            }
        }
    }
}


