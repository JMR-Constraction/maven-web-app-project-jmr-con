node {
def myhome= tool name: "mvn"
    withCredentials([usernamePassword(credentialsId: 'tomcat-credentials', passwordVariable: 'TOMCAT_PASSWORD', usernameVariable: 'TOMCAT_USERNAME')]) {
        def tomcatURL = 'http://43.205.210.172:8080/manager'
        def warFile = '/var/lib/jenkins/workspace/deops/target/maven-web-application.war'
        def appName = 'maven-web-application'

       stage('git checkout')
	{
     git branch: 'feature1', credentialsId: 'c12bc9a7-aa90-4822-9c84-77625cfaa039', url: 'https://github.com/JMR-Constraction/maven-web-app-project-jmr-con.git'
	}
	stage('mav build')
	{
     sh "${myhome}/bin/mvn clean package"
	}
	stage('mvn install')
	{
      sh "${myhome}/bin/mvn clean install"
	}
	stage('SonarQube report')
	{
      sh "${myhome}/bin/mvn clean install sonar:sonar"
	}
	stage('nexus report')
	{
      sh "${myhome}/bin/mvn clean deploy sonar:sonar"
	}

        stage('Deploy to Tomcat') {
            // Deploy the WAR to Tomcat using curl with Jenkins credentials
            sh """
                curl -u ${TOMCAT_USERNAME}:${TOMCAT_PASSWORD} \
                    -T ${warFile} \
                    "${tomcatURL}/deploy?path=/${appName}&update=true"
            """
        }
    }
}
