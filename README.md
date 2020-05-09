# wordpress_k8s

Pipeline (Jenkinsfile) to create a wordpress deployment based on customized images from Gitlab, it comes with other projects : [deploy_jenkins](https://github.com/ghassencherni/deploy_jenkins), [terraform_aws_eks](https://github.com/ghassencherni/terraform_aws_eks) and [wp_custom_docker](https://github.com/ghassencherni/wp_custom_docker).

Please start loocking in [deploy_jenkins](https://github.com/ghassencherni/deploy_jenkins) First.

## Getting Started

This Jenkinsfile deploys wordpress cluster on EKS, it creates:

- Persistent Volume for /var/www/html data

- ConfigMap that contains all RDS connection parameters ( triggered by terraform_aws_eks ) 

- The LB service : EKS allows to automtically build an ALB in front of the worker nodes ( HTTP listener only at this version ) 

- Create the Wordpress deployment ( images "ghassen-devopstt" are pulled from Gitlab Registry )


## Author Information

This script  was created by [Ghassen CHARNI](https://github.com/ghassencherni/)

