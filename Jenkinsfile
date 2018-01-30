node {
    def String dbRollbackConfigFile = 'db/rollback.tag.yml'

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

                echo "Rollback started"
                def datas = readYaml file: dbRollbackConfigFile

                echo datas
                                liquibaseRollback(changeLogFile: 'db/master.xml',
                                     testRollbacks: false,
                                     databaseEngine: 'Postgres',
                                     liquibasePropertiesPath: 'db/liquibase.properties',
                                     rollbackToTag: datas.rollbackToTag)
                echo "Rollback success"
                
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