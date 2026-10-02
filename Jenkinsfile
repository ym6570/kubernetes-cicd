pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // 1단계: 깃허브에서 이 프로젝트 폴더를 통째로 다운로드(Checkout) 합니다.
                checkout scm
            }
        }
        stage('Validate') {
            steps {
                // 2단계: K8s 설계도(yaml) 문법이 맞는지 가상으로 테스트(--dry-run) 해봅니다.
                sh 'kubectl apply --dry-run=client -f k8s/'
            }
        }
        stage('Deploy') {
            steps {
                // 3단계: 순서대로 안전하게 K8s에 배포합니다 (에러 방지 처리 완료).
                sh 'kubectl apply -f k8s/namespace.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
        stage('Verify') {
            steps {
                // 4단계: 배포가 잘 되었는지 상태를 꼼꼼히 확인합니다.
                sh 'kubectl rollout status deployment/cicd-web -n cicd-lab'
                sh 'kubectl get pods -n cicd-lab'
                sh 'kubectl get service -n cicd-lab'
            }
        }
    }
}