// Declarative pipelines must be enclosed with a "pipeline" directive.
pipeline {
    // This line is required for declarative pipelines. Just keep it here.
    agent any

    // This section contains environment variables which are available for use in the
    // pipeline's stages.
    environment {
      region = "us-east-1"
      docker_repo_uri = "854570149286.dkr.ecr.us-east-1.amazonaws.com/endpoint-decoupled"
      task_def_arn = ""
      cluster = ""
      exec_role_arn = ""
      commit_id = "0001"

      IMAGE_VERSION = "kube_v0.0.${env.BUILD_ID}"
      IMAGE_NAME = "${env.ACR_LOGINSERVER}/grupohuman/khor2/endpoint:${env.IMAGE_VERSION}"
      DEPLOYMENT_NAME = "endpoint-khor2-deployment"
      CONTAINER_NAME = "endpoint"
      RESOURCE_GROUP = "${env.AZURE_RESOURCE_GROUP}"
      CLUSTER_NAME = "${env.KHOR_KUBERNETES_CLUSTER}"
    }
    
    // Here you can define one or more stages for your pipeline.
    // Each stage can execute one or more steps.
    stages {
      // This is a stage.
      stage('Build') {
        steps {
          // Get SHA1 of current commit
          // script {
            // commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
          // }
          // Build the Docker image
          sh "docker build -t ${docker_repo_uri}:${commit_id} ."
          // Get Docker login credentials for ECR
          sh "aws ecr get-login --no-include-email --region ${region} | sh"
          // Push Docker image
          sh "docker push ${docker_repo_uri}:${commit_id}"
          // Clean up
          sh "docker rmi -f ${docker_repo_uri}:${commit_id}"
        }
      }
    }
}
