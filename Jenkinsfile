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
                sh 'kubectl config current-context'
                sh 'kubectl get nodes'
            }
        }
        stage('Deploy Angular') {
            steps {
                // Aplica el despliegue de Angular
                sh 'kubectl apply -f k8s/angular-deployment.yaml'
            }
        }
        stage('Deploy Spring Boot') {
            steps {
                // Aplica el despliegue de Spring Boot
                sh 'kubectl apply -f k8s/springboot-deployment.yaml'
            }
        }
        stage('Wait for Pods') {
            steps {
                // Espera a que los pods estén listos
                sh 'kubectl wait --for=condition=ready pod -l app=angular --timeout=120s'
                sh 'kubectl wait --for=condition=ready pod -l app=springboot --timeout=120s'
            }
        }
        stage('Show Status') {
            steps {
                // Muestra el estado de los despliegues y servicios
                sh 'kubectl get deployments'
                sh 'kubectl get services'
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