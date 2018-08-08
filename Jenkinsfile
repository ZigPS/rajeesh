pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
			 script {
          def datas = readYaml file: 'release.yml'
          echo "Got version as ${datas.data.build} "
	 echo "Got version as ${datas.data.test} "
			def first =  ${datas.data.test}
			echo first 
			 	         
		    if( ${datas.data.build} == 'maven')
		    {
		    withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn -B -V -U -e clean package'
                }
		    }else
		    {
		    echo "in build else"
		    }
			 } 
            }
        }

        stage ('Testing Stage') {
		
		steps {
		junit allowEmptyResults: true, testResults: '**/target/**/TEST*.xml'
		       }     
        }
	
	    stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
			    withSonarQubeEnv('Sonar') {
			    	sh '/scratch/EDP/sonar-scanner-3.2.0.1227-linux/bin/sonar-scanner'
			    }
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
               
                    sh 'cp /root/.jenkins/workspace/pipeline-example/target/my-app-1.0-SNAPSHOT.war /scratch/EDP/apache-tomcat-8.5.32/webapps'
                
            }
        }
    }
}
