pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "nalupaws-web"
    }

    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    set -e

                    echo ">> Verificando red externa 'cicd'..."
                    if ! docker network inspect cicd >/dev/null 2>&1; then
                        echo ">> La red 'cicd' no existe. Creándola..."
                        docker network create cicd
                    else
                        echo ">> La red 'cicd' ya existe."
                    fi

                    echo ">> Deteniendo contenedores anteriores..."
                    docker compose down --remove-orphans

                    echo ">> Construyendo y levantando nalupaws-web..."
                    docker compose up -d --build

                    docker compose ps
                '''
            }
        }
    }

    post {
        always {
            echo '>> Limpiando imágenes huérfanas...'
            sh 'docker image prune -f'
        }
        success {
            echo '✅ NALUPAWS desplegado correctamente en el puerto 8080.'
        }
        failure {
            echo '❌ El despliegue de NALUPAWS ha fallado. Revisa los logs.'
        }
    }
}
