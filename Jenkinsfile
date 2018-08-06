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
		
		junit allowEmptyResults: true, testResults: '**/target/**/TEST*.xml'
		            
        }


        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}
