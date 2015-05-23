#################################################################################
# Dockerfile image that provides JBoss jBPM Workbench 6.2.0.Final Showcase
#
# The web application binaries are downloaded from JBoss Nexus Repository.
#
# The Docker image generated name/tag is "jboss/jbpm-workbench-showcase:6.2.0.Final"
#################################################################################

####### BASE ############
FROM jboss/jbpm-workbench:6.2.0.Final

####### MAINTAINER ############
MAINTAINER "Roger Martinez" "romartin@redhat.com"

####### jBPM Workbench CUSTOM CONFIGURATION ############
# There is an issue in Docker with ADD command.
# When a file is added into the container's filesystem, the file owner is always root, instead of the current running user.
# See https://github.com/docker/docker/issues/5110
# The workaround is doing a chown using root user and then switchback to jboss user.
USER root
ADD etc/standalone-full-jbpm.xml $JBOSS_HOME/standalone/configuration/standalone-full-jbpm.xml
ADD etc/jbpm-users.properties $JBOSS_HOME/standalone/configuration/jbpm-users.properties
ADD etc/jbpm-roles.properties $JBOSS_HOME/standalone/configuration/jbpm-roles.properties
RUN chown jboss:jboss $JBOSS_HOME/standalone/configuration/standalone-full-jbpm.xml && \ 
chown jboss:jboss $JBOSS_HOME/standalone/configuration/jbpm-users.properties && \ 
chown jboss:jboss $JBOSS_HOME/standalone/configuration/jbpm-roles.properties

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
USER jboss

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./standalone.sh", "-b", "0.0.0.0", "--server-config=standalone-full-jbpm.xml"]
