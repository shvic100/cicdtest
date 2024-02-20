pipeline {
  agent any
  stages {
      stage('git scm update') {
          steps {
          git url: 'https://github.com/shvic100/cicdtest.git', branch: 'main'
          }
      }
      stage('docker build and push') {
          steps {
              sh '''
              sudo docker build -t shvic/cicdtest:green .
              sudo docker push shvic/cicdtest:green
              '''
          }
      }
      stage('deploy kubernetes') {
          steps {
              sh '''
              ansible master -m shell -a 'export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl create deploy myweb2 --image=shvic/cicdtest:green'
              ansible master -m shell -a 'export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl expose deploy myweb2 --type="LoadBalancer" --port=80 --target-port=80 --protocol="TCP"'
              '''
          }
      }
  }
}
