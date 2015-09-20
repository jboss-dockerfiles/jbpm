###########################################################################
# Dockerfile that provides the image for JBoss jBPM Workbench 6.3.0.Final
###########################################################################

####### BASE ############
FROM jboss/wildfly:8.2.0.Final

####### MAINTAINER ############
MAINTAINER "Roger Martinez" "romartin@redhat.com"

####### ENVIRONMENT ############
ENV JBOSS_BIND_ADDRESS 0.0.0.0
ENV KIE_REPOSITORY https://repository.jboss.org/nexus/content/groups/public-jboss
ENV KIE_VERSION 6.3.0.Final
ENV KIE_CLASSIFIER wildfly8
ENV KIE_CONTEXT_PATH jbpm-console
ENV DASHBUILDER_CONTEXT_PATH dashbuilder
# Do NOT use demo neither examples by default in this image (no internet connection required).
ENV KIE_DEMO false
ENV JAVA_OPTS -XX:MaxPermSize=256m -Xms256m -Xmx512m

####### JBPM-WB ############
RUN curl -o $HOME/$KIE_CONTEXT_PATH.war $KIE_REPOSITORY/org/kie/kie-wb-distribution-wars/$KIE_VERSION/kie-wb-distribution-wars-$KIE_VERSION-$KIE_CLASSIFIER.war && \
unzip -q $HOME/$KIE_CONTEXT_PATH.war -d $JBOSS_HOME/standalone/deployments/$KIE_CONTEXT_PATH.war &&  \
touch $JBOSS_HOME/standalone/deployments/$KIE_CONTEXT_PATH.war.dodeploy &&  \ 
rm -rf $HOME/$KIE_CONTEXT_PATH.war

####### JBPM DASHBUILDER ############
RUN curl -o $HOME/$DASHBUILDER_CONTEXT_PATH.war $KIE_REPOSITORY/org/jbpm/dashboard/jbpm-dashboard-distributions/$KIE_VERSION/jbpm-dashboard-distributions-$KIE_VERSION-$KIE_CLASSIFIER.war && \
unzip -q $HOME/$DASHBUILDER_CONTEXT_PATH.war -d $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war &&  \
touch $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war.dodeploy &&  \ 
rm -rf $HOME/$DASHBUILDER_CONTEXT_PATH.war

####### MVN REPO ############
# https://bugzilla.redhat.com/show_bug.cgi?id=1263738
RUN mkdir -p $HOME/.m2/repository/org/guvnor/guvnor-asset-mgmt-project/$KIE_VERSION && \
curl -o $HOME/.m2/repository/org/guvnor/guvnor-asset-mgmt-project/$KIE_VERSION/guvnor-asset-mgmt-project-$KIE_VERSION.jar $KIE_REPOSITORY/org/guvnor/guvnor-asset-mgmt-project/$KIE_VERSION/guvnor-asset-mgmt-project-$KIE_VERSION.jar

####### INTER WAR DEPENDENCY DESCRIPTOR FOR WILDFLY8 ############
USER root
ADD etc/jboss-all.xml $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war/META-INF/jboss-all.xml
RUN chown jboss:jboss $JBOSS_HOME/standalone/deployments/*

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
USER jboss

####### EXPOSE INTERNAL JBPM GIT PORT ############
EXPOSE 8001

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./standalone.sh", "-b", "$JBOSS_BIND_ADDRESS", "-Dorg.kie.demo=$KIE_DEMO", "-Dorg.kie.example=$KIE_DEMO", "-c", "standalone-full.xml"]