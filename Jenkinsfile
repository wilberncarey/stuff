pipeline {
 agent none
  parameters {
    choice(name: 'Release',
      choices: 'YES\nNO',
      description: 'Is this a candidate for Release?')
    string(name: 'TAGPARAM',
      defaultValue: 'Release Version?',
      description: 'What is the tag or release Version?')
  }
  
 stages {
      
      
  stage('Dev Deploy'){

    agent any
      steps {
        echo "Is this A release Candidate? ${params.Release}"
        echo "Release Version is : ${params.TAGPARAM}"
        echo "pull SVN"
        echo "build"
        echo "push snaphost to NEXUS"
        echo "undeploy"
        echo "stop servers"
        echo "deploy"
        echo "start servers"
      }
      
    }
  stage('Release request') { 
          when {
            // Only  run if a "YES" is requested
               expression { Release == 'YES' }
            }
     agent none
      steps {
       emailext(subject: "Release version \"$params.TAGPARAM\" to Nexus as a candidate?", replyTo: 'wilberncarey@hotmail.com', to: 'wilberncarey@hotmail.com', body: '$PROJECT_NAME - Build # $BUILD_NUMBER - Release is ready to promote to nexus as a candidate as a release candidate.<br/> <br/> Check Jenkins <a href="${BUILD_URL}/input">approval link</a> to approve the Release to be moved to Nexus.<br/> If you cannot connect to the build server, check the attached logs.<br/> <br/> --<br/> Following is the last 100 lines of the log.<br/> <br/> --LOG-BEGIN--<br/> <pre style=\'line-height: 22px; display: block; color: #333; font-family: Monaco,Menlo,Consolas,"Courier New",monospace; padding: 10.5px; margin: 0 0 11px; font-size: 13px; word-break: break-all; word-wrap: break-word; white-space: pre-wrap; background-color: #f5f5f5; border: 1px solid #ccc; border: 1px solid rgba(0,0,0,.15); -webkit-border-radius: 4px; -moz-border-radius: 4px; border-radius: 4px;\'> ${BUILD_LOG, maxLines=100, escapeHtml=true} </pre> --LOG-END--', from: 'wilberncarey@hotmail.com')
     
       script {
         env.CRE_REL = input message: "Release $params.TAGPARAM as production candidate ?", ok: 'Submit!',
            parameters: [choice(name: 'Create Release', choices: 'YES\nNO', description: 'proceed with yes will create a release')]
                }
        echo "${env.CRE_REL}"
      }
      
}
    
  stage('Create Release') { 
          when {
            // Only  run if a "YES" is requested
               expression { Release == 'YES' && CRE_REL == 'YES' }
            }
     agent none
      steps {
       
       echo "create $params.TAGPARAM tag"
       echo "pull from tag $params.TAGPARAM"
       echo "build release $params.TAGPARAM"
       echo "push release $params.TAGPARAM to nexus"
      }
      
    }  
    
  stage('QA Deploy Approval') { 
        when {
        // Only  run if a "YES" is requested
           expression { Release == 'YES' && CRE_REL == 'YES' }
            }
     agent none
      steps {
        emailext(subject: "Deploy version \"$params.TAGPARAM\" to QA?", replyTo: 'wilberncarey@hotmail.com', to: 'wilberncarey@hotmail.com', body: '$PROJECT_NAME - Build # $BUILD_NUMBER - Deploy is ready for QA.<br/> <br/> Check Jenkins <a href="${BUILD_URL}/input">approval link</a> to approve the QA Deploy.<br/> If you cannot connect to the build server, check the attached logs.<br/> <br/> --<br/> Following is the last 100 lines of the log.<br/> <br/> --LOG-BEGIN--<br/> <pre style=\'line-height: 22px; display: block; color: #333; font-family: Monaco,Menlo,Consolas,"Courier New",monospace; padding: 10.5px; margin: 0 0 11px; font-size: 13px; word-break: break-all; word-wrap: break-word; white-space: pre-wrap; background-color: #f5f5f5; border: 1px solid #ccc; border: 1px solid rgba(0,0,0,.15); -webkit-border-radius: 4px; -moz-border-radius: 4px; border-radius: 4px;\'> ${BUILD_LOG, maxLines=100, escapeHtml=true} </pre> --LOG-END--', from: 'wilberncarey@hotmail.com')
     
       script {
         env.DEP_QA = input message: "Deploy Release $params.TAGPARAM to QA?", ok: 'Submit!',
            parameters: [choice(name: 'Deploy to QA', choices: 'YES\nNO', description: 'proceed with yes will Deploy to QA')]
                }
        echo "${env.DEP_QA}"
      }
     }
      
    

  stage('QA Deploy') { 
          when {
               // Only  run if a "YES" is requested
               expression { Release == 'YES' && CRE_REL == 'YES' && DEP_QA == 'YES' }
            }
     agent any
      steps {
       echo "pull Release $params.TAGPARAM from Nexus"
       echo "Deploy to QA"
       echo "ripple start QA"
      }
      
    }  

  stage('Prod Deploy Approval') { 
           when {
           // Only  run if a "YES" is requested
           expression { Release == 'YES' && CRE_REL == 'YES' && DEP_QA == 'YES' }
           }
     agent none
      steps {
        emailext(subject: "Deploy version \"$params.TAGPARAM\" to PROD?", replyTo: 'wilberncarey@hotmail.com', to: 'wilberncarey@hotmail.com', body: '$PROJECT_NAME - Build # $BUILD_NUMBER - Deploy is ready for PROD.<br/> <br/> Check Jenkins <a href="${BUILD_URL}/input">approval link</a> to approve the PROD Deploy.<br/> If you cannot connect to the build server, check the attached logs.<br/> <br/> --<br/> Following is the last 100 lines of the log.<br/> <br/> --LOG-BEGIN--<br/> <pre style=\'line-height: 22px; display: block; color: #333; font-family: Monaco,Menlo,Consolas,"Courier New",monospace; padding: 10.5px; margin: 0 0 11px; font-size: 13px; word-break: break-all; word-wrap: break-word; white-space: pre-wrap; background-color: #f5f5f5; border: 1px solid #ccc; border: 1px solid rgba(0,0,0,.15); -webkit-border-radius: 4px; -moz-border-radius: 4px; border-radius: 4px;\'> ${BUILD_LOG, maxLines=100, escapeHtml=true} </pre> --LOG-END--', from: 'wilberncarey@hotmail.com')
     
       script {
         env.DEP_PROD = input message: "Deploy Release $params.TAGPARAM to PROD?", ok: 'Submit!',
            parameters: [choice(name: 'Deploy to PROD', choices: 'YES\nNO', description: 'proceed with yes will Deploy to PROD')]
                }
                
        emailext(subject: "Deploy version \"$params.TAGPARAM\" to PROD?", replyTo: 'wilberncarey@hotmail.com', to: 'wilberncarey@hotmail.com', body: '$PROJECT_NAME - Build # $BUILD_NUMBER - Deploy is ready for PROD.<br/> <br/> Check Jenkins <a href="${BUILD_URL}/input">approval link</a> to approve the PROD Deploy.<br/> If you cannot connect to the build server, check the attached logs.<br/> <br/> --<br/> Following is the last 100 lines of the log.<br/> <br/> --LOG-BEGIN--<br/> <pre style=\'line-height: 22px; display: block; color: #333; font-family: Monaco,Menlo,Consolas,"Courier New",monospace; padding: 10.5px; margin: 0 0 11px; font-size: 13px; word-break: break-all; word-wrap: break-word; white-space: pre-wrap; background-color: #f5f5f5; border: 1px solid #ccc; border: 1px solid rgba(0,0,0,.15); -webkit-border-radius: 4px; -moz-border-radius: 4px; border-radius: 4px;\'> ${BUILD_LOG, maxLines=100, escapeHtml=true} </pre> --LOG-END--', from: 'wilberncarey@hotmail.com')
     
       script {
         env.DEP_OPS_PROD = input message: "Deploy Release $params.TAGPARAM to PROD?", ok: 'Submit!',
            parameters: [choice(name: 'Deploy to PROD', choices: 'YES\nNO', description: 'proceed with yes will Deploy to PROD')]
                }
        echo "${env.DEP_OPS_PROD}"
      }
      
      
    } 
 
  stage('Prod Deploy') { 
          when {
            // Only  run if a "YES" is requested
               expression { Release == 'YES' && CRE_REL == 'YES' && DEP_QA == 'YES' && DEP_PROD == 'YES' && DEP_OPS_PROD == 'YES'}
            }
     agent any
      steps {
       echo "pull Release $params.TAGPARAM from Nexus"
       echo "Deploy to PROD"
       echo "ripple start PROD"
      }
      
    } 
}
}
