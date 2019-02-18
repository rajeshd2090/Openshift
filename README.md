# Openshift-

# Install Jenkins:
    oc new-app --template=openshift/jenkins-persistent -e INSTALL_PLUGINS=configuration-as-code,configuration-as-code-support,matrix-auth:2.3 CASC_JENKINS_CONFIG=https://raw.githubusercontent.com/rajeshd2090/Openshift/master/Jenkins.yaml -n=<Project-Name>
    
# Create and Start Pipeline with command
    oc new-app https://github.com/rajeshd2090/Openshift --strategy=pipeline -n=demo-devops

# OR Use below two step and build-config YAML

  * Create Pipeline

    oc apply -f https://raw.githubusercontent.com/rajeshd2090/Openshift/master/build-config.yaml -n=Project-Name
   
  * Start Pipeline
    oc start-build <Pipeline-Name> -n=Project-Name
