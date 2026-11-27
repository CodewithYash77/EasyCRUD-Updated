 sqp_f3129547c330f58d3ffbfdf813b30128f267689c

			mvn clean verify sonar:sonar \
			  -Dsonar.projectKey=studentapp \
			  -Dsonar.projectName='studentapp' \
			  -Dsonar.host.url=http://13.207.1.131:9000 \
			  -Dsonar.token=sqp_f3129547c330f58d3ffbfdf813b30128f267689c
			  
			  
Dockerhub:- dckr_pat_NqP3x84zi6IEcpniODuwAsU1pvo

IS:- 885d423a-69e4-4042-8908-e85140191166

frontend :- pipeline {
    agent { label 'CloudBlitz-Agent' }

    stages {
        stage('Clone Repo') {
            steps {
                git url: "https://github.com/CodewithYash77/EasyCRUD-Updated.git", branch: "main"
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh "docker build --no-cache -t trainwithyash77/new-easycurd_frontend ./frontend"
            }
        }

        stage('Docker Hub Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh 'echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin'
                    sh 'docker push trainwithyash77/new-easycurd_frontend'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_acess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws eks update-kubeconfig --region ap-south-1 --name cloudblitz-cluster
                        echo "=== Nodes ==="
                        kubectl get nodes
                        kubectl apply -f frontend/frontend-pod.yaml
                        kubectl apply -f frontend/frontend-svc.yaml
                        echo "=== Pods ==="
                        kubectl get pods
                        echo "=== Services ==="
                        kubectl get svc
                    '''
                }
            }
        }
    }
}


backend:-pipeline {
    agent { label 'CloudBlitz-Agent' }

    stages {
        stage('Clone Repo') {
            steps {
                git url: "https://github.com/CodewithYash77/EasyCRUD-Updated.git", branch: "main"
            }
        }

        stage('Build Backend Image') {
            steps {
                sh "docker build --no-cache -t trainwithyash77/new-easycurd_backend ./backend"
            }
        }

        stage('Docker Hub Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh 'echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin'
                    sh 'docker push trainwithyash77/new-easycurd_backend'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_acess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws eks update-kubeconfig --region ap-south-1 --name cloudblitz-cluster
                        echo "=== Nodes ==="
                        kubectl get nodes
                        kubectl delete pod backend --ignore-not-found=true
                        kubectl apply -f backend/backend-pod.yaml
                        kubectl apply -f backend/backend-svc.yaml
                        echo "=== Pods ==="
                        kubectl get pods
                        echo "=== Services ==="
                        kubectl get svc
                    '''
                }
            }
        }
    }
}


frontend:--with a adding a pom.xml in script

pipeline {
    agent { label 'CloudBlitz-Agent' }

    stages {
        stage('Clone Repo') {
            steps {
                git url: "https://github.com/CodewithYash77/EasyCRUD-Updated.git", branch: "main"
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh "docker build --no-cache -t trainwithyash77/new-easycurd_frontend ./frontend"
            }
        }

        stage('Docker Hub Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh 'echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin'
                    sh 'docker push trainwithyash77/new-easycurd_frontend'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_acess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws eks update-kubeconfig --region ap-south-1 --name cloudblitz-cluster
                        echo "=== Nodes ==="
                        kubectl get nodes

                        cat <<'EOF' | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: easycrud-frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: easycrud-frontend
  template:
    metadata:
      labels:
        app: easycrud-frontend
    spec:
      containers:
        - name: frontend
          image: trainwithyash77/new-easycurd_frontend:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: easycrud-frontend-svc
spec:
  type: NodePort
  selector:
    app: easycrud-frontend
  ports:
    - port: 80
      targetPort: 80
EOF

                        echo "=== Pods ==="
                        kubectl get pods
                        echo "=== Services ==="
                        kubectl get svc
                    '''
                }
            }
        }
    }
}



DNS output:- http://13.235.78.148.nip.io:31460
