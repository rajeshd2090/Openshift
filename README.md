# Openshift-

# Install Jenkins:
    oc new-app --template=openshift/jenkins-persistent -e INSTALL_PLUGINS=configuration-as-code,configuration-as-code-support,matrix-auth:2.3 CASC_JENKINS_CONFIG=https://raw.githubusercontent.com/rajeshd2090/Openshift-/master/Jenkins.yaml

# Create Pipeline

    oc apply -f https://raw.githubusercontent.com/rajeshd2090/Openshift-/master/build-config.yaml
   
# Start Pipeline
    oc start-build <Pipeline-Name>
