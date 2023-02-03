pipeline {
	agent any
	tools {
		maven 'maven'
	}
	parameters {
		choice(name: 'Branch', choices['DEV', 'SIT', 'UAT', 'PRE-PROD'], description: 'Pick the Right Branch to build the Artifacts')
	}
	stages {
    stage('Initialize'){
      steps{
        echo "PATH = ${M2_HOME}/bin:${PATH}"
        echo "M2_HOME = /opt/apache-maven-3.8.5"
      }
		}
		stage('Checkout') {
			steps {
			checkout([$class: 'GitSCM', branches: [[name: '${params.Branch}']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-access', url: 'https://github.com/Reshmasthan/simple-java-maven-app.git']]])
			}
		}
		stage ('Build') {
			steps {
			sh 'mvn -B -DskipTests clean package'		
			}
		}		
	}
	post {
        success {
          emailext(
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Development Promoted to Master",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Development Promoted to Master":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
            to: "vali2575ulla@gmail.com"
          )
        }
   
        failure {
        emailext(
            subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
            body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
            to: "vali2575ulla@gmail.com"
        )
        }
	}
}
