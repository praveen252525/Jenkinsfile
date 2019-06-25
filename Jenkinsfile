pipeline {
    /* insert Declarative Pipeline here */
    // agent  {  label  'DevOps-Cloud-Node2'  }
    /* agent any */
    /* agent none */
    /* agent { node { label 'DevOps-Cloud-Node2' } } */
    agent {
        node {
            label 'DevOps-Cloud-Node2'
            /* customWorkspace '/home/jenkins/customworkspace' */
        }
    }
   triggers {
        cron('* * * H/1 *')
        pollSCM('* * * H/1 *')
        /* upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS) */
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2', daysToKeepStr: '2', artifactDaysToKeepStr: '2'))
        /* checkoutToSubdirectory('customworkspace') */
        disableConcurrentBuilds()
        disableResume()
        overrideIndexTriggers(true)
        preserveStashes(buildCount: 2)
        quietPeriod(5)
        retry(2)
        skipDefaultCheckout(false)
        skipStagesAfterUnstable()
        timeout(time: 3, unit: 'MINUTES')
        timestamps ()
        parallelsAlwaysFailFast()
    }
     parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'Jenkins', defaultValue: 'Jenkins', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')

        file(name: "FILE", description: "Choose a file to upload")
    }
    stages  {
        stage('Sequential Parameter Section') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
            stages {
                stage('Input Section') {
                    input {
                        message "Should we continue?"
                        ok "Yes, we should."
                        submitter "alice,bob"
                        parameters {
                            string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                        }
                    }
                    steps {
                        echo "Hello, ${PERSON}, nice to meet you."
                    }
                }
                stage('In Sequential 2') {
                    steps {
                        echo "In Sequential 2"
                    }
                }
                stage('Parallel In Sequential') {
                    beforeAgent true
                    failFast true
                    parallel {
                        stage('Branch master and master2') {
                            when {
                                expression { BRANCH_NAME ==~ /(master|master2)/ }
                                anyOf {
                                    environment name: 'DEPLOY_TO', value: 'master'
                                    environment name: 'DEPLOY_TO', value: 'master2'
                                }
                            }
                            agent  {  label  'DevOps-Cloud-Node1'  }
                            steps {
                                echo 'Deploying to master OR master2'
                            }
                         }
                         stage('when Master2 Branch Section') {
                             when {
                                 beforeInput true
                                 allOf {
                                     branch 'master2'
                                     branch 'master3'
                                 }
                                 input {
                                 message "Deploy to production?"
                                 id "simple-input"
                                 }
                                 steps {
                                     echo 'Deploying to master2 and master3 environments'
                                 }
                             }
                         }
                    }
                }
            }
        }
    }
    post  {
        always  {
            echo 'this  can run always'
        }
        success  {
            echo 'this can run only if build is successful'
        }
        failure  {
            echo 'this can run if build gets failure'
        }
        unstable  {
            echo 'this  comes when we see unstable build'
        }
        fixed  {
            echo 'run is successful and the previous run failed or was unstable'
        }
        changed  {
            echo 'this comes when previous build is unsucessful but now the build is successful'
        }
        regression  {
                echo 'run’s status is failure, unstable, or aborted and the previous run was successful'
        }
        aborted  {
                echo 'run has an "aborted" status, usually due to the Pipeline being manually aborted'
        }
        unsuccessful  {
                echo 'stage’s run has not a "success" status'
        }
        cleanup  {
                echo 'after every other post condition has been evaluated, regardless of the Pipeline or stage’s status'
        }
    }
}
