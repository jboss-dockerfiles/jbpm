#################################################################################
# Dockerfile image that provides JBoss jBPM Workbench 6.2.0.Final Showcase
#
# The web application binaries are downloaded from JBoss Nexus Repository.
#
# The Docker image generated name/tag is "jboss/jbpm-workbench-showcase:6.2.0"
#################################################################################

####### BASE ############
FROM jboss/jbpm-workbench:6.2.0

####### MAINTAINER ############
MAINTAINER "Roger Martinez" "romartin@redhat.com"

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
USER jboss

####### jBPM Workbench CUSTOM CONFIGURATION ############
# There is an issue in Docker with ADD command.
# When a file is added into the container's filesystem, the file owner is always root, instead of the current running user.
# See https://github.com/docker/docker/issues/5110
# The workaround is doing a chown using root user and then switchback to jboss user.
ADD etc/standalone-full-jbpm.xml /opt/jboss/wildfly/standalone/configuration/standalone-full-jbpm.xml
ADD etc/jbpm-users.properties /opt/jboss/wildfly/standalone/configuration/jbpm-users.properties
ADD etc/jbpm-roles.properties /opt/jboss/wildfly/standalone/configuration/jbpm-roles.properties
USER root
RUN chown jboss:jboss /opt/jboss/wildfly/standalone/configuration/standalone-full-jbpm.xml && \ 
chown jboss:jboss /opt/jboss/wildfly/standalone/configuration/jbpm-users.properties && \ 
chown jboss:jboss /opt/jboss/wildfly/standalone/configuration/jbpm-roles.properties
# Switchback to jboss user
USER jboss

####### RUNNING JBPM-WB ############
WORKDIR /opt/jboss/wildfly/bin/
CMD ["./standalone.sh", "-b", "0.0.0.0", "--server-config=standalone-full-jbpm.xml"]