pipeline {
        agent any

        environment {
            
            CONTAINER_ID = ''
            SUM_PY_PATH = 'C:/Users/charp/desktop/tp-4-devops/sum.py'
            DIR_PATH = 'C:/Users/teo/desktop'
            TEST_FILE_PATH = 'C:/Userscharp/Desktop/tp-4-devops/test_variables.txt'
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
        
        stages('Build, ça arrive ')
            {
        steps{
            echo "Construction de l'image docker"
            sh "docker build -t container-tp-4-devops-image ${env.DIR_PATH}"
            }
        }

        stages('Run') {
                steps {
                    echo "Lancement du conteneur Docker..."

                    // Lancer le conteneur et récupérer son ID
                    script {
                        def output = sh(script: "docker run -dit container-tp-4-devops-image", returnStdout: true)
                        def lines = output.split('\n')
                        env.CONTAINER_ID = lines[-1].trim()
                        echo "Conteneur lancé avec ID : ${env.CONTAINER_ID}"
                    }
                }
                stage('Test') {
                steps {
                    script {
                        def testLines = readFile(env.TEST_FILE_PATH).split('\n')
                        
                        for (line in testLines) {
                            if (line.trim() == '') continue  // Ignore les lignes vides

                            def vars = line.trim().split(' ')
                            def arg1 = vars[0]
                            def arg2 = vars[1]
                            def expectedSum = vars[2].toFloat()

                            // Exécution du script dans le conteneur Docker
                            def output = sh(
                                script: "docker exec ${env.CONTAINER_ID} python /app/sum.py ${arg1} ${arg2}",
                                returnStdout: true
                            )

                            def result = output.split('\n')[-1].trim().toFloat()

                            if (result == expectedSum) {
                                echo "Test réussi : ${arg1} + ${arg2} = ${result}"
                            } else {
                                error "Test échoué : ${arg1} + ${arg2} = ${result}, attendu : ${expectedSum}"
                            }
                        }
                    }
                }
            }
        
        }
    }

}