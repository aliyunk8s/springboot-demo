pipeline {
    agent  any
  }

  stages {
    stage('Git代码拉取'){       
      steps{
        git branch: 'master', credentialsId: 'tests',  url: 'https://github.com/zbbkeepgoing/springboot-demo.git'              
		} 
    }   

    stage('Build'){
	  steps{
        sh "mvn clean compile"
        sh "mvn package"
        }
    }
    
    stage('Junit'){
      steps{
        junit allowEmptyResults: true, keepLongStdio: true, testResults: 'target/surefire-reports/*.xml'
        }
    }
    
    stage('jacoco'){
      steps{
        jacoco exclusionPattern: 'src/main/java', inclusionPattern: '**/classes'
        }
    }
    
    //这里需要注意的配置
    //  Path to exec files: **/jacoco.exec 可执行文件路径
    //  Path to class directories: 这个配置的是源代码编译后的字节码目录，也就是classes目录不是test-classes目录，如果有多个可以指定多个
    //  Path to source directories: 这个配置的是源代码的目录，也就是src/main/java目录，如果有多个可以指定多个
    
     stage('SonarQube') {
        withSonarQubeEnv('SonarQube') {
            sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner \
            -Dsonar.projectKey=springboot-demo \
            -Dsonar.projectName=springboot-demo \
            -Dsonar.sources=.\
            -Dsonar.host.url=https://sonar.gwmdevops.com \
			-Dsonar.login=admin \
			-Dsonar.password=v3KQ4nnnSjcT3K7P \
            -Dsonar.language=java \
            -Dsonar.sourceEncoding=UTF-8"
        }
    }
}
