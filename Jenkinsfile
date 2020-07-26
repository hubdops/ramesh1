node{
    agent {label 'kubetcat'}
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/RameshEY/spring-boot-mongo-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t testdockerramesh/spring-boot-mongo .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "docker login -u testdockerramesh -p ${DOKCER_HUB_PASSWORD}"
        }
        sh 'docker push testdockerramesh/spring-boot-mongo'
     }
	 
	 stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "springBootMongo.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }
     
     	 
	  /**
      stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f pringBootMongo.yml'
      } **/
     
}
