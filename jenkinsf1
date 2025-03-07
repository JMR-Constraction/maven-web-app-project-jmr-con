node{
def myhome= tool name: "mvn"
	
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
	stage('deploy to tomcat')
	{
	sh"""
	curl -u admin:admin123 \
	--upload-file /var/lib/jenkins/workspace/deops/target/maven-web-application.war \
	"http://43.205.210.172:8080/manager/text/deploy?path=/maven-web-apllication&update=ture"
	"""
    }
}
