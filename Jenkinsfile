node {
  try {
    stage('Checkout') {
      checkout scm
      echo "Branch: ${env.BRANCH_NAME}"
    }
    stage('Create new image'){
      if(env.BRANCH_NAME == 'main'){
        docker.build("wkhtmltopdf","--build-arg BUILD_ID=1.0.${BUILD_ID} -f Dockerfile .")
        sh 'docker tag wkhtmltopdf localhost:15000/wkhtmltopdf:12.5.1'
        sh 'docker push localhost:15000/wkhtmltopdf'
        sh 'docker rmi -f wkhtmltopdf localhost:15000/wkhtmltopdf'
      }
    }
    stage('Deploy new image'){
      if(env.BRANCH_NAME == 'main'){
        build job: 'Infrastructure Changes', parameters: [string(name: 'Source', value: 'wkhtmltopdf')]
      }
    }
  }
  catch (err) {
    throw err
  }
}