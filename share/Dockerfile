ARG SHARE_TAG
FROM alfresco/alfresco-share:${SHARE_TAG}

ARG TOMCAT_DIR=/usr/local/tomcat

# Server data
ARG SERVER_NAME

# Install modules and addons
RUN mkdir -p $TOMCAT_DIR/amps
COPY modules/amps $TOMCAT_DIR/amps
COPY modules/jars $TOMCAT_DIR/webapps/share/WEB-INF/lib

# Install Share modules in specific order
RUN java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
    $TOMCAT_DIR/amps/becpg-share-23.2.2.33.amp $TOMCAT_DIR/webapps/share -nobackup -force && \
    java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
    $TOMCAT_DIR/amps/becpg-project-share-23.2.2.33.amp $TOMCAT_DIR/webapps/share -nobackup -force && \
    java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
    $TOMCAT_DIR/amps/becpg-designer-share-23.2.2.33.amp $TOMCAT_DIR/webapps/share -nobackup -force && \
    java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
    $TOMCAT_DIR/amps/becpg-plm-share-23.2.2.33.amp $TOMCAT_DIR/webapps/share -nobackup -force

# Increase default cacheMaxSize
RUN sed -i "s|<Context>|<Context>\n<Resources cacheMaxSize=\"51200\"/> |g" $TOMCAT_DIR/conf/context.xml

# Fix for https://github.com/Alfresco/acs-community-packaging/issues/367 in Share 6.2.0
COPY web-extension/share-config-custom-dev.xml $TOMCAT_DIR/shared/classes/alfresco/web-extension/
