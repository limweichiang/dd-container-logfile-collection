FROM httpd:2.4

# Setting up Fluentbit(td-agent-bit) on a Debian-based container (httpd is
# based on Debian Buster Slim base image). The APT source set in fluentbit.list
# is for Debian Buster; Change that as required depending on your container 
# base image.
RUN apt-get update && apt-get install -y wget gnupg &&  wget -qO - https://packages.fluentbit.io/fluentbit.key | apt-key add -
COPY ./fluentbit.list /etc/apt/sources.list.d/
RUN apt-get update && apt-get install -y td-agent-bit
COPY ./td-agent-bit.conf /etc/td-agent-bit/

# Optional for demo purposes. You should not need this if your container
# already has an existing log file directory with log files
RUN mkdir -p /var/log/apache2
COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf

# Step 1: Sleep 10 seconds and startup Fluentbit(td-agent-bit) - Make sure all
#         log files are created before we start scanning, sometimes Fluentbit
#         misses files that don't exist before it starts.
# Step 2: Run the actual container command (Taken from CMD in the base
#         httpd:2.4 image)
CMD sleep 10 && /opt/td-agent-bit/bin/td-agent-bit -c /etc/td-agent-bit/td-agent-bit.conf & httpd-foreground
