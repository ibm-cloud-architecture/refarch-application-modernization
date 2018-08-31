FROM websphere-liberty

RUN mkdir /opt/ibm/wlp/usr/shared/resources/mq
COPY wmq.jmsra.rar /opt/ibm/wlp/usr/shared/resources/mq
COPY run.sh /usr/local/bin
RUN chmod +x /usr/local/bin/run.sh
COPY server.xml /opt/ibm/wlp/usr/servers/defaultServer
COPY MQLibertyConversionApp.war /opt/ibm/wlp/usr/servers/defaultServer/apps
ENV LICENSE accept
RUN /opt/ibm/wlp/bin/installUtility install defaultServer --acceptLicense

EXPOSE 9080 9443

CMD /opt/ibm/wlp/bin/server run defaultServer