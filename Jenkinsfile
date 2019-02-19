node {
	def MAVEN_HOME = tool "Maven_HOME"
    def JAVA_HOME = tool "JAVA_HOME"
    env.PATH="${env.PATH}:${MAVEN_HOME}/bin:${JAVA_HOME}/bin"
    
    def GIT_URL='https://github.com/sourabhgupta385/starwars'
	def OS_PROJECT_NAME='coolstore-ui-cicd'
	def REPO_NAME='starwars'
       
    stage('First Time Deployment'){
        script{
            openshift.withCluster() {
                openshift.withProject("${OS_PROJECT_NAME}") {
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
    }
    
	stage ('Checkout') {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${GIT_URL}"]]])
    }
    
	stage('Packaging') {   
        sh 'mvn -DskipTests package'     
    }
    
	stage("Dev - Building Application"){
        script{
            openshift.withCluster() {
                openshift.withProject("${OS_PROJECT_NAME}"){
                    openshift.startBuild("${REPO_NAME}","--wait").narrow('svc').expose()
		
                }
            }
        }
	}
   
        
}
