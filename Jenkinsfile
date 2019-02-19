node {
    
    def MAVEN_HOME = tool "Maven_HOME"
    def JAVA_HOME = tool "JAVA_HOME"
    env.PATH="${env.PATH}:${MAVEN_HOME}/bin:${JAVA_HOME}/bin"
    
    def GIT_URL='https://github.com/sourabhgupta385/starwars'
	def OS_PROJECT_NAME='startwars-app'
	def REPO_NAME='starwars'
       
    stage('Create Image Builder'){
        script{
            openshift.withCluster() {
                    def bcSelector = openshift.selector( "bc", "${REPO_NAME}")
                    def bcExists = bcSelector.exists()
                    if (!bcExists) {
                        openshift.newApp("redhat-openjdk18-openshift:1.1~${GIT_URL}","--strategy=source")
		
                    } else {
			//openshift.create([ kind : "RoleBinding", metadata: [name:"default_edittt"], roleRef: [name: "edit"], subjects:[kind: "ServiceAccount",name: "defaultxyz"] ]) 
                        sh 'echo build config already exists'  
                    } 
                }
            }
        }
    
    
    stage ('Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${GIT_URL}"]]])
    }    
	
    stage('Packaging') {   
        sh 'mvn -DskipTests package'     
    }
    
    stage("Building Image"){
        script{
            openshift.withCluster() {
                      openshift.startBuild("${REPO_NAME}","--wait")
	            }
            }
         }
	}
   stage('Promote to DEV') {      
        script {
          openshift.withCluster() {
		   openshift.tag("${REPO_NAME}:latest", "${OS_PROJECT_NAME}-dev/${REPO_NAME}:dev")
		}        
      }
    }
    stage('Create DEV') {
           openshift.withCluster() {
		  openshift.withProject("${OS_PROJECT_NAME}-dev") {
		   def dcSelector = openshift.selector( "dc", "${OS_PROJECT_NAME}-dev")
                    def dcExists = dcSelector.exists()
                    if (!dcExists) {
			     openshift.newApp("${REPO_NAME}:dev", "--name=${REPO_NAME}-dev").narrow('svc').expose()
		    }
		  }
             }     
    }
        
}
