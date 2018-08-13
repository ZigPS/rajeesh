pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
			 script {
          def datas = readYaml file: 'release.yml'
          echo "Got tool as ${datas.data.build} "
	  		 	         
		    if( datas.data.build == 'maven')
		    {
		    withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn -B -V -U -e clean'
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
			script {
          def datas = readYaml file: 'release.yml'
          
	 echo "Got tool as ${datas.data.test} "
	 
	   if( datas.data.test == 'junit')
		    {
		junit allowEmptyResults: true, testResults: '**/target/**/TEST*.xml'
			}else
			{
					    echo "in test else"			
			}	
					}		
		       }     
        }
	
	    stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
				script {
          def datas = readYaml file: 'release.yml'   
	 echo "Got tool as ${datas.data.quality} "
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
				 if( datas.data.quality == 'sonar')
		    {
			    withSonarQubeEnv('Sonar') {
			    	sh '/scratch/EDP/sonar-scanner-3.2.0.1227-linux/bin/sonar-scanner'
			    }
			}else
			{
				 echo "in quality else"		
			}
				}
            }
        }
	stage ('Archival Stage') {
		
		steps {
				script {
          def datas = readYaml file: 'release.yml'
          echo "Got tool as ${datas.data.package} "
	  if( datas.data.package == 'maven')
		    {
		withMaven(maven : 'maven_3_5_3') {
                    sh 'mvn package -DskipTests'
                }
			}else{
				 echo "in package else"
			    nexusPublisher nexusInstanceId: 'Nexus2', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'war/target/packagedPipieline.war']], mavenCoordinate: [artifactId: 'packagedPipieline.war', groupId: 'com.mycompany.app', packaging: 'war', version: '2.12']]]

				}	}
		       }     
        }
	    
         stage ('Deployment Stage') {
            steps {
               script {
          def datas = readYaml file: 'release.yml'
       echo "Got tool as ${datas.data.deploy} "
				if( datas.data.deploy == 'copy')
		    {
                    sh 'cp /root/.jenkins/workspace/pipeline-example/target/my-app-1.0-SNAPSHOT.war /scratch/EDP/apache-tomcat-8.5.32/webapps'
			}else
			{
				echo "in deploy else"
			}
			   }
            }
        }
    }
}
