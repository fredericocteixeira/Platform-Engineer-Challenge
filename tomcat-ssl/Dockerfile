FROM oraclelinux:9

RUN yum install -y wget openssl tar

RUN wget https://download.oracle.com/java/22/latest/jdk-22_linux-x64_bin.tar.gz \
    && tar xvfz jdk-22_linux-x64_bin.tar.gz \
    && mv jdk-22.0.2 /usr/lib/jvm/ \
    && rm -rf jdk-22_linux-x64_bin.tar.gz

ENV JAVA_HOME=/usr/lib/jvm/jdk-22.0.2
ENV PATH=$PATH:JAVA_HOME/bin

RUN wget https://downloads.apache.org/tomcat/tomcat-10/v10.1.28/bin/apache-tomcat-10.1.28.tar.gz \
    && tar xvzf apache-tomcat-10.1.28.tar.gz \
    && mv apache-tomcat-10.1.28 /usr/local/tomcat \
    && rm -rf apache-tomcat-10.1.28.tar.gz

RUN wget https://tomcat.apache.org/tomcat-10.0-doc/appdev/sample/sample.war -O /usr/local/tomcat/webapps/sample.war

RUN mkdir -p /usr/local/tomcat/conf/ssl

ENV SSL_PATH=/usr/local/tomcat/conf/ssl

ENV CATALINA_HOME=/usr/local/tomcat

WORKDIR /

COPY private_key.pem $SSL_PATH/

COPY public_key.pem $SSL_PATH/

RUN openssl req -new -key $SSL_PATH/private_key.pem -out $SSL_PATH/certificate.csr -subj "/C=PT/ST=State/L=City/O=Organization/OU=Unit/CN=localhost" -config /etc/pki/tls/openssl.cnf -passin pass:"changeit"

RUN openssl x509 -req -in $SSL_PATH/certificate.csr -signkey $SSL_PATH/private_key.pem -out $SSL_PATH/certificate.crt -passin pass:"changeit"

RUN openssl pkcs12 -export -in $SSL_PATH/certificate.crt -inkey $SSL_PATH/private_key.pem -out $SSL_PATH/keystore.p12 -name tomcat -CAfile $SSL_PATH/ca.crt -caname root -passin pass:"changeit" -passout pass:"changeit"

WORKDIR $PATH

RUN keytool -importkeystore \
    -srckeystore $SSL_PATH/keystore.p12 \
    -destkeystore $SSL_PATH/keystore.jks \
    -srcstoretype PKCS12 \
    -srcstorepass changeit \
    -deststorepass changeit \
    -srcalias tomcat \
    -destalias tomcat \
    -srckeypass changeit \
    -destkeypass changeit \
    -noprompt

RUN chmod 600 $SSL_PATH/keystore.p12 && chmod 600 $SSL_PATH/keystore.jks

COPY server.xml $CATALINA_HOME/conf/

COPY sample.war $CATALINA_HOME/webapps/

EXPOSE 4041

CMD ["/usr/local/tomcat/bin/catalina.sh", "run"]
