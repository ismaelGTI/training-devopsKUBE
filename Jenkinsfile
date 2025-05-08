pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde Git
                git branch: 'main', url: 'https://github.com/ismaelGTI/training-devopsKUBE.git'
            }
        }
        stage('Verify Kubernetes Connection') {
            steps {
                // Verifica la conexión con el clúster
                bat 'kubectl config current-context'
                bat 'kubectl get nodes'
            }
        }
        stage('Deploy Angular') {
            steps {
                // Aplica el despliegue de Angular
                bat 'kubectl apply -f k8s/angular-deployment.yaml'
            }
        }
        stage('Deploy Spring Boot') {
            steps {
                // Aplica el despliegue de Spring Boot
                bat 'kubectl apply -f k8s/springboot-deployment.yaml'
            }
        }
        stage('Wait for Pods') {
            steps {
                // Espera a que los pods estén listos
                bat 'kubectl wait --for=condition=ready pod -l app=angular --timeout=120s'
                bat 'kubectl wait --for=condition=ready pod -l app=springboot --timeout=120s'
            }
        }
        stage('Show Status') {
            steps {
                // Muestra el estado de los despliegues y servicios
                bat 'kubectl get deployments'
                bat 'kubectl get services'
            }
        }
    }
    post {
        always {
            // Mensaje final con las URLs de acceso
            echo "Pipeline completed. Access Angular at http://localhost:30080 and Spring Boot at http://localhost:30081"
        }
    }
}