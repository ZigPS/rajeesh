pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
			withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn -B -V -U -e clean package'
                }
               
            }
        }

        stage ('Testing Stage') {
		
		steps {
		junit allowEmptyResults: true, testResults: '**/target/**/TEST*.xml'
		       }     
        }

	stage ('Archival Stage') {
		
		steps {
		withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn package -DskipTests'
                }
		       }     
        }
	    
         stage ('Deployment Stage') {
            steps {
               
                    sh 'cp /root/.jenkins/workspace/pipeline-example/target/my-app-1.0-SNAPSHOT.jar /scratch/EDP/apache-tomcat-8.5.32/webapps'
                
            }
        }
    }
}
