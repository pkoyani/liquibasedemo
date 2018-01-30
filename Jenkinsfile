node {
    def String dbRollbackConfigFile = 'db/rollback.tag.yaml'

    currentBuild.result = "SUCCESS"
	try { 
     stage('Checkout') { //(1)
            checkout scm
        }

	stage('Build') { //(2)

                liquibaseUpdate(changeLogFile: 'db/master.xml',
                                  testRollbacks: false,
                                  databaseEngine: 'Postgres',
                                  liquibasePropertiesPath: 'db/liquibase.properties'
                                )


                def rollbackInfo = readYaml file: dbRollbackConfigFile,text: ""
                if(rollbackInfo.isRollbackEnable)
                {
                    echo rollbackInfo.rollbackToTag
                    liquibaseRollback(changeLogFile: 'db/master.xml',
                                        testRollbacks: false,
                                        databaseEngine: 'Postgres',
                                        liquibasePropertiesPath: 'db/liquibase.properties',
                                        rollbackToTag: rollbackInfo.rollbackToTag)
                }

        }

    stage('Clean-up') { //(2)
        }
	} catch (Exception e) {
        currentBuild.result = "FAILURE"
        println("catch exeption. currentBuild.result: ${currentBuild.result}")
        throw e
    }
}