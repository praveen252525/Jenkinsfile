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
        stage('Parameters Section') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
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
        stage('If-Else Block') {
            if (env.BRANCH_NAME == 'master') {
                echo 'I only execute if block'
            }
            else {
                echo 'I execute elsewhere block'
            }
        }
        stage('Parallel Stage') {
            failFast true
            parallel {
                stage('Branch A') {
                    when {
                        beforeInput true
                        allOf {
                            branch 'master'
                        }
                    }
                    input {
                    message "Deploy to production?"
                    id "simple-input"
                    }
                    steps {
                        echo 'Deploying to master2 environments'
                    }
                }
                stage('Branch B') {
                    when {
                        anyOf {
                            branch 'master2'
                            buildingTag()
                            changelog '.*^\\[DEPENDENCY\\] .+$'
                            changeset glob: "ReadMe.*", caseSensitive: true
                            changeRequest target: 'master2'
                            environment name: 'DEPLOY_TO', value: 'production'
                            equals expected: 2, actual: currentBuild.number
                            expression { return params.DEBUG_BUILD }
                            tag "release-*"
                            triggeredBy 'SCMTrigger'
                            triggeredBy 'TimerTrigger'
                            triggeredBy 'UpstreamCause'
                            triggeredBy cause: "UserIdCause", detail: "vlinde"
                        }
                    }
                    steps {
                        echo "On Branch B"
                    }
                }
                stage('Branch c') {
                    when {
                        not {
                            branch 'master1'
                        }
                    }
                    steps {
                        script {
                            def browsers = ['chrome', 'firefox']
                            for (int i = 0; i < browsers.size(); ++i) {
                                echo "Testing the ${browsers[i]} browser"
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
