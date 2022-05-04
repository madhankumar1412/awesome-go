#!/usr/bin/env groovy

//@Library("global-jenkins-library@master") _
//@Library("rbi-jenkins-library@main") _

pipeline {
    agent any
    
  options {
  buildDiscarder logRotator(daysToKeepStr: '', numToKeepStr: '12')
  }

  environment { 
      ARTIFACTORY_CREDS = "aws-artifactory"
      ARTIFACTORY_REPO   = "rbi-docker.us-artifactory.cicd.cloud.fpdev.io"  
      VERSION = "5.4.22"
       }

    stages {

          stage("SCM Checkout"){
           environment {
             REPO_NAME = sh(script: 'echo $GIT_URL | rev | cut -d \'/\' -f1 | rev | cut -d \'.\' -f1|tr -d \'\n\'', returnStdout: true)
           }
                steps{
                   // git url: "$GIT_URL", branch: "$BRANCH_NAME", credentialsId: 'ghe-jenkins-bot'
                   echo "Building ${env.TAG_NAME}"
                  script {
                      GIT_TAG = sh(script: 'git tag --contains|head -n 1|tr -d \'\n\'', returnStdout: true)
                      print "SCM Checkout: GIT_TAG is $GIT_TAG"
                      REPO_NAME = REPO_NAME.toLowerCase()
                    
                      }
                  }
              }

        /*stage('Unit Testing') {
           agent {
               docker {
                   image 'golang'
                   args  '-u root'
                   
                 }
             }
           steps {
               sh 'go test -v ./... '
               
                 }
             }*/

        stage("Build cluster admin Container Image"){
	when {
		changeset "**/chart/**"
		}
            steps {
                script { 
                        sh "echo rbc-cluster-admin"
                        echo "Commit=${env.GIT_COMMIT}"
                        echo "Building ${env.TAG_NAME}"
                        //echo "TAG_NAME is ${TAG_NAME}"
                        //sh " yum install wget -y"
                        //sh " wget https://github.com/mikefarah/yq/releases/download/v4.24.5/yq_linux_amd64 -O /usr/local/bin/yq &> /dev/null 
                        //sh  "chmod +x /usr/local/bin/yq"
			//sh "yq -i '.rbi-cluster-admin.image.tag = \"${VERSION}\"' ./chart/values.yaml"
			 sh "sed 's/unix/linux/g' ./chart/file.txt"
			
			sh "git config --global user.name \"jenkins\" && git config --global user.email jenkins@frbi.dev"
                        sh "git commit -am \"JENKINS:Auto-commit\" && git push https://github.com/madhan1412/awesome-go.git"

                       /* if (env.TAG_NAME == null && env.BRANCH_NAME == 'gitcheckin') {
		                           print "inside if env.TAG_NAME = ${env.TAG_NAME}"

                                VERSION = getVersion()
                                repo_child = "rbc-cluster-admin"     
                                //sh "yq -i '.rbi-cluster-admin.image.tag = \"${VERSION}\"' ./chart/values.yaml" 
                                //containerBuild(repo_child,VERSION)
                                //gitCheckin(REPO_NAME,env.TAG_NAME,VERSION)
		                      }

                        else if (env.TAG_NAME != null) {
                               print "inside else if  env.TAG_NAME  == ${env.TAG_NAME}"
                               repo_child = "rbc-cluster-admin"
                              // containerBuild(repo_child,env.TAG_NAME)
                               //sh "yq -i '.rbi-rbc-cluster-admin.image.tag = \"${env.TAG_NAME}\"' ./chart/values.yaml" 

                          }       
 
                        
                      else {
                              echo "No main branch or any tag found"
                       }*/
                                                   
                       

              }
            }
          }

         
         }

        post {

          always { 
                  cleanWs()
              }

          success {
              echo 'This build has success.'
            }

            unsuccessful {
              echo 'This build has failed.'
            }        
        
          }

      }

       
      
  
