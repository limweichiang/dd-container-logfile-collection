# The httpd base image is used for this example.
# CHANGE THIS to your production app image label.
FROM httpd:2.4

# Setting up Fluentbit(td-agent-bit) on a Debian-based container (httpd is
# based on Debian Buster Slim base image). The APT source set in fluentbit.list
# is for Debian Buster; Change that as required depending on your container 
# base image.
RUN apt-get update && apt-get install -y wget gnupg &&  wget -qO - https://packages.fluentbit.io/fluentbit.key | apt-key add -
COPY ./fluentbit.list /etc/apt/sources.list.d/
RUN apt-get update && apt-get install -y td-agent-bit
COPY ./td-agent-bit.conf /etc/td-agent-bit/

# Optional for demo purposes with httpd. Comment out these lines when building
# the production image, which should already have a log file directory.
RUN mkdir -p /var/log/apache2
COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf

# Step 1: Sleep 10 seconds and startup Fluentbit(td-agent-bit) - This ensures
#         all log files are created by the application. Sometimes Fluentbit
#         misses files that don't exist before it starts.
# Step 2: Run the actual container command ('httpd-foreground' is taken from
#         the CMD directive in the base httpd:2.4 image)
CMD sleep 10 && /opt/td-agent-bit/bin/td-agent-bit -c /etc/td-agent-bit/td-agent-bit.conf & httpd-foreground
