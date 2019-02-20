# Openshift-

# Build Project

    oc new-projects <PROJECT-NAME>
    
# DEV Project
 
    oc new-projects <PROJECT-NAME>
    
    oc policy add-role-to-user edit system:serviceaccount:<PROJECT-NAME>:jenkins
    

# Install Jenkins:
    oc new-app --template=openshift/jenkins-persistent -e INSTALL_PLUGINS=configuration-as-code,configuration-as-code-support,matrix-auth:2.3,sonar,nodejs CASC_JENKINS_CONFIG=https://raw.githubusercontent.com/rajeshd2090/Openshift/master/Jenkins.yaml -n=<Project-Name>
    
    
    oc new-app --template=openshift/jenkins-ephemeral -e INSTALL_PLUGINS=matrix-auth,configuration-as-code-support,sonar  CASC_JENKINS_CONFIG=https://raw.githubusercontent.com/rajeshd2090/Openshift/master/Jenkins.yaml
    
# Create and Start Pipeline with command
    oc new-app https://github.com/rajeshd2090/Openshift --strategy=pipeline -n=demo-devops

# OR Use below two step and build-config YAML

  * Create Pipeline

    oc apply -f https://raw.githubusercontent.com/rajeshd2090/Openshift/master/build-config.yaml -n=Project-Name
   
  * Start Pipeline
    oc start-build <Pipeline-Name> -n=Project-Name
