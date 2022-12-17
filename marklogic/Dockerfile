FROM marklogicdb/marklogic-db:10.0-9.5-centos-1.0.0

RUN sudo rm -rf /tmp/filebeat-access; mkdir /tmp/filebeat-access
RUN sudo rm -rf /tmp/filebeat-error; mkdir /tmp/filebeat-error
# copy project files
USER marklogic_user
COPY marklogic/filebeat-7.17.7-linux-x86_64.tar.gz /tmp
COPY marklogic/filebeat-access/filebeat.yml /tmp/filebeat-access/
COPY marklogic/filebeat-error/filebeat.yml /tmp/filebeat-error/
USER root
COPY marklogic/start-marklogic.sh /usr/local/bin

