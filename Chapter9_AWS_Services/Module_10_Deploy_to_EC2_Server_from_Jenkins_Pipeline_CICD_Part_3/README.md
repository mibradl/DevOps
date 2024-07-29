# Chapter9
This module we will deploy to EC2 server from Jenkins pipeline - Part 3

# Name: Deploy to EC2 server from Jenkins Pipeline

# Description: 

AWS and Jenkins Complete Pipeline - Part 3

    Setting the Image name dynamically

        Increment Version ---> Build App --->   Build and Push Image ---> Deploy to EC2 ---> Commit version bump
        in pom.xml            with new version  build and store new ver     deploy new ver
                                                 docker image ver           with docker-compose


    stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        } 


    This is a change in your Git respository code

    Next Pipeline must start with the new version!

       stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'gitlab-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh 'git remote set-url origin https://$USER:$PASS@gitlab.com/drmibradl/jenkins-shared-library.git'
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                }
            }
        }

        This is a real life example of a complete pipeline


        Keep in mind the stage order

        When stage X depends on the output of stage Y, then stage Y needs to be place before

        When stage fails, the next set of stages are skipped

        Extract into groovy sripts

        Extract into Shared Library

    
    
    
Container Orchestration

    Deployment looks different for more advanced use cases

        Container Orchestration instead of Docker Compose

# Usage


    