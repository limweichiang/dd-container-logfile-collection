# Update this to use the appropriate parent image.
FROM jboss/wildfly:21.0.1.Final

# Elevating to root privilege for OS level changes.
USER root

# Setting up Fluentbit(td-agent-bit) on a Centos-based container
# (jboss/wildfly is based on a Centos 7 base image). The YUM repo in
# td-agent-bit.repo is for Centos 7; Change that as required depending
# on your container base image.
COPY ./td-agent-bit.repo /etc/yum.repos.d/
RUN yum install -y td-agent-bit
COPY ./td-agent-bit.conf /etc/td-agent-bit/

# Dropping back to jboss privilege, root privilege not required from here.
USER jboss

# Step 1: Sleep 10 seconds and startup Fluentbit(td-agent-bit) - Make sure all
#         log files are created before we start scanning, sometimes Fluentbit
#         misses files that don't exist before it starts.
# Step 2: Run the actual container command (Taken from CMD in the base
#         jboss/wildfly image)
CMD sleep 10 && /opt/td-agent-bit/bin/td-agent-bit -c /etc/td-agent-bit/td-agent-bit.conf & /opt/jboss/wildfly/bin/standalone.sh -b 0.0.0.0
