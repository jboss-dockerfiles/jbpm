###########################################################################
# Dockerfile image that provides JBoss jBPM Workbench 6.2.0.Final
#
# The web application binaries are downloaded from JBoss Nexus Repository.
#
# The Docker image generated name/tag is "jboss/jbpm-workbench:6.2.0.Final"
###########################################################################

####### BASE ############
FROM jboss/wildfly:8.1.0.Final

####### MAINTAINER ############
MAINTAINER "Roger Martinez" "romartin@redhat.com"

####### ENVIRONMENT ############
ENV KIE_REPOSITORY https://repository.jboss.org/nexus/content/groups/public-jboss/
ENV JBPM_VERSION 6.2.0.Final
ENV JBPM_CLASSIFIER wildfly8
ENV JBPM_CONTEXT_PATH jbpm-console
ENV DASHBUILDER_CONTEXT_PATH dashbuilder
ENV JAVA_OPTS -XX:MaxPermSize=256m -Xms256m -Xmx512m
USER root

####### JBPM-WB ############
RUN curl -o /opt/jboss/$JBPM_CONTEXT_PATH.war $KIE_REPOSITORY/org/kie/kie-wb-distribution-wars/$JBPM_VERSION/kie-wb-distribution-wars-$JBPM_VERSION-$JBPM_CLASSIFIER.war && \ 
unzip -q /opt/jboss/$JBPM_CONTEXT_PATH.war -d $JBOSS_HOME/standalone/deployments/$JBPM_CONTEXT_PATH.war &&  \ 
touch $JBOSS_HOME/standalone/deployments/$JBPM_CONTEXT_PATH.war.dodeploy &&  \ 
rm -rf /opt/jboss/$JBPM_CONTEXT_PATH.war

####### JBPM DASHBUILDER ############
RUN curl -o /opt/jboss/$DASHBUILDER_CONTEXT_PATH.war $KIE_REPOSITORY/org/jbpm/dashboard/jbpm-dashboard-distributions/$JBPM_VERSION/jbpm-dashboard-distributions-$JBPM_VERSION-$JBPM_CLASSIFIER.war && \ 
unzip -q /opt/jboss/$DASHBUILDER_CONTEXT_PATH.war -d $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war &&  \ 
touch $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war.dodeploy &&  \ 
rm -rf /opt/jboss/$DASHBUILDER_CONTEXT_PATH.war

####### INTER WAR DEPENDENCY DESCRIPTOR FOR WILDFLY8 ############
ADD etc/jboss-all.xml $JBOSS_HOME/standalone/deployments/$DASHBUILDER_CONTEXT_PATH.war/META-INF/jboss-all.xml
RUN chown jboss:jboss $JBOSS_HOME/standalone/deployments/*

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
USER jboss

####### EXPOSE INTERNAL JBPM GIT PORT ############
EXPOSE 8001

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./standalone.sh", "-b", "0.0.0.0", "--server-config=standalone-full.xml"]