node {
    currentBuild.result = "SUCCESS"
	try { 
     stage('Checkout') { //(1)
            echo "Checkout"
            checkout scm
        }

	stage('Build') { //(2)
                 echo "Build"

                 liquibaseUpdate(changeLogFile: 'db/master.xml',
                                  testRollbacks: false,
                                  databaseEngine: 'Postgres',
                                  liquibasePropertiesPath: 'db/liquibase.properties'
                                )
        }

    stage('Clean-up') { //(2)
                 echo "Clean-up-Done"
        }
	} catch (Exception e) {
        currentBuild.result = "FAILURE"
        println("catch exeption. currentBuild.result: ${currentBuild.result}")
        throw e
    }
}