node {

  git 'https://github.com/ghassencherni/notes_k8s.git'
  withCredentials([usernamePassword(credentialsId: 'aws_credentials', usernameVariable: 'ACCESS_KEY', passwordVariable: 'SECRET_ACCESS')])
{

  if(action == 'Deploy Notes') {


    stage('Getting "config" and "rds_conn_configmap.yaml"') {
      copyArtifacts filter: 'service-notes.yaml, rds_conn_configmap.yaml, config', fingerprintArtifacts: true, projectName: 'terraform_aws_eks_psg', selector: upstream(fallbackToLastSuccessful: true)
    }
    stage('Create the ConfigMap') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          export KUBECONFIG=config
          kubectl apply -f rds_conn_configmap.yaml
         """
      }
    stage('Create/update the Deployment') {
      sh """
          export IMAGE_VERSION='${notes_image_version}'
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          export KUBECONFIG=config
          envsubst < deployment-notes.yaml | kubectl apply -f -
         """
      }
    stage('Create the LB Service') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          sleep 20
          export KUBECONFIG=config
          kubectl apply -f service-notes.yaml
        """
      }
    stage('Get the LB notes URL') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          sleep 20
          export KUBECONFIG=config
          kubectl get service/notes-service
         """
      }
    }


if(action == 'Destroy Notes') {

    stage('Delete the LB Service') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          export KUBECONFIG=config
          kubectl delete service/notes-service
         """
      }
    stage('Delete the Deployment') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          export KUBECONFIG=config
          kubectl delete deployment/notes-deployment
         """
      }
     stage('Delete the ConfigMap') {
      sh """
          export AWS_ACCESS_KEY_ID='$ACCESS_KEY'
          export AWS_SECRET_ACCESS_KEY='$SECRET_ACCESS'
          export KUBECONFIG=config
          kubectl delete configmap/rds-conn
         """
      }
    }
 
   }
 
 }
