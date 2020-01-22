# This Dockerfile is based on the M4EU/Deegree cookbook from 
# https://geusgitlab.geus.dk/m4eu/2019-ORAMA-v2.xx/blob/master/00-cookbook-m4eu/G4.1.01%20M4EU%20Tool%20Stack%20guidelines/G4.1.01.04%20Apache%20Tomcat%20&%20Deegree%20install%20guidelines.pdf
#
# First we state which parent image we are building on
# Bionic is Ubuntu 18.04 (LTS releases supported for 5 years)
# Ubuntu LTS-releases are even numbers - april release. Next would be 20.04 release in April 2020.
FROM ubuntu:bionic 
MAINTAINER bjorn.ove.grotan@ngu.no

# Generate and set LANG environment
ENV LANG=en_US.UTF8

# Update and upgrade system
RUN apt-get -qq update --fix-missing && apt-get -qq --yes upgrade

RUN apt-get install -y apt-utils wget git openssh-server
RUN apt-get install -y locales unzip zip curl

RUN locale-gen en_US.UTF-8  
RUN locale-gen nn_NO.UTF-8
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8  

RUN update-locale LANG=nn_NO.UTF-8

# Install OpenJDK 8 and Tomcat8
RUN apt-get update -y
RUN apt-get install -y openjdk-8-jdk
RUN apt-get install -y tomcat8

# Download Deegree (renamed m4eu)
RUN curl "https://geusgitlab.geus.dk/m4eu/2019-ORAMA-v2.xx/raw/master/06-deegree3/deegree-webservices-3.4.3/m4eu.zip?inline=false" > m4eu.zip
RUN unzip m4eu.zip
RUN mv m4eu.war /var/lib/tomcat8/webapps

# Download M4EU Deegree workspace
RUN mkdir /opt/deegree/
RUN curl "https://geusgitlab.geus.dk/m4eu/2019-ORAMA-v2.xx/raw/master/10%20Deegree%20setup/M4EU/v1.1.2/.deegree/m4eu%20v1.1.2.zip?inline=false" > m4eu-v1.1.2.zip
RUN unzip "m4eu-v1.1.2.zip"
RUN mv m4eu\ v1.1.2/m4eu /opt/deegree

# Set Deegree environment variables. Can Deegree use ENVIRONMENT variables instead of hardcoded configuration files for passwords etc?
# If not, presume we have a non-versioned file jdbc.xml containing our jdbc configuration 
ENV DEEGREE_WORKSPACE_ROOT=/opt/deegree/
COPY jdbc.xml /opt/deegree/m4eu/jdbc/m4eu.xml
RUN chown -R tomcat8 /opt/deegree/m4eu

# Tell Docker to expose port 8080 to the host machine
EXPOSE 8080

# Start Tomcat and if successfull, continously tail logfile
ENV CATALINA_HOME=/usr/share/tomcat8
ENV CATALINA_BASE=/var/lib/tomcat8
ENV CATALINA_OUT=/var/log/tomcat8/catalina.out
ENV CATALINA_SH=$CATALINA_HOME/bin/startup.sh
CMD $CATALINA_SH start && tail -f $CATALINA_OUT