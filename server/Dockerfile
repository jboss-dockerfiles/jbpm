###########################################################################
# Dockerfile that provides the image for JBoss jBPM Server Full 7.11.0.Final
###########################################################################

####### BASE ############
FROM jboss/wildfly:11.0.0.Final

####### MAINTAINER ############
MAINTAINER "Michael Biarnes Kiefer" "mbiarnes@redhat.com"

####### ENVIRONMENT ############
ENV JBOSS_BIND_ADDRESS 0.0.0.0
ENV KIE_REPOSITORY https://download.jboss.org/jbpm/release
ENV KIE_VERSION 7.11.0.Final
ENV KIE_CLASSIFIER wildfly11
ENV KIE_CONTEXT_PATH jbpm-console
ENV EXTRA_OPTS -Dorg.jbpm.ht.admin.group=admin -Dorg.uberfire.nio.git.ssh.host=$JBOSS_BIND_ADDRESS

####### JBPM-WB ############
RUN curl -o $HOME/jbpm-server-dist.zip $KIE_REPOSITORY/$KIE_VERSION/jbpm-server-$KIE_VERSION-dist.zip && \
unzip -o -q jbpm-server-dist.zip -d $JBOSS_HOME &&  \
rm -rf $HOME/jbpm-server-dist.zip

####### CONFIGURATION ############
USER root
ADD etc/start_jbpm-wb.sh $JBOSS_HOME/bin/start_jbpm-wb.sh
ADD etc/update_db_config.sh $JBOSS_HOME/bin/update_db_config.sh
RUN chown jboss:jboss $JBOSS_HOME/standalone/deployments/*
RUN chown jboss:jboss $JBOSS_HOME/bin/start_jbpm-wb.sh
RUN chown jboss:jboss $JBOSS_HOME/bin/update_db_config.sh

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
USER jboss

####### EXPOSE INTERNAL JBPM GIT PORT ############
EXPOSE 8001

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./start_jbpm-wb.sh"]
