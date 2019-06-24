pipeline {
    /* insert Declarative Pipeline here */
    /* agent  {  label  'DevOps-Cloud-Node2'  } */
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
        cron('* * * * *')
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
