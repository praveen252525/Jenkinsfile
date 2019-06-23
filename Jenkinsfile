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
    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2', daysToKeepStr: '2', artifactDaysToKeepStr: '2'))
        checkoutToSubdirectory('/workspace/customworkspace1')
        disableConcurrentBuilds()
        disableResume()
        overrideIndexTriggers(true)
        preserveStashes(buildCount: 2)
        quietPeriod(5)
        retry(2)
        skipDefaultCheckout()
        skipStagesAfterUnstable()
        timeout(time: 3, unit: 'MINUTES')
        timestamps ()
        parallelsAlwaysFailFast()
    }
    stages  {
        stage('Build')  {
            steps  {
                sh  'mvn --version'
                sh  'java -version'
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
