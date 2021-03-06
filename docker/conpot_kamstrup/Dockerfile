FROM alpine
MAINTAINER MO

# Include dist
ADD dist/ /root/dist/

# Define Environment Variables
ENV CONPOT_TEMPLATE="kamstrup_382" CONPOT_LOG="/var/log/conpot/conpot_kamstrup.log" CONPOT_CONFIG="/etc/conpot/conpot_kamstrup.cfg"

# Setup apt
RUN apk -U add bash \
               build-base \
               file \
               git \
               libev \
               libtool \
               libxslt \
               libxslt-dev \
               mariadb-dev \
               mariadb-client-libs \
               pkgconfig \
               python \
               python-dev \
			   py-gevent \
			   py-snmp \			   
               py-cffi && \

# Setup ConPot
    git clone https://github.com/mushorg/conpot /opt/conpot_kamstrup/ && \
    cd /opt/conpot_kamstrup/ && \
    git checkout d97a68a054e4fe42ff90293188a5702ce8ab09a3 && \
    cp /root/dist/requirements.txt /opt/conpot_kamstrup/ && \
    python setup.py install && \
    cd / && \
    rm -rf /opt/conpot_kamstrup /tmp/* /var/tmp/* && \

# Setup user, groups and configs
    addgroup -g 2000 conpot_kamstrup && \
    adduser -S -s /bin/bash -u 2000 -D -g 2000 conpot_kamstrup && \
    mkdir -p /etc/conpot /var/log/conpot && \
    mv /root/dist/conpot.cfg /etc/conpot/conpot_kamstrup.cfg && \
    mv /root/dist/kamstrup_382/template.xml /usr/lib/python2.7/site-packages/Conpot-0.5.1-py2.7.egg/conpot/templates/kamstrup_382/ && \

# Clean up
    apk del build-base \
            file \
            git \
            libev \
            libtool \
            libxslt-dev \
            mariadb-dev \
            pkgconfig \
            python-dev \	
            py-cffi && \
    rm -rf /root/* && \
    rm -rf /var/cache/apk/*

# Run supervisor upon container start
CMD /usr/bin/conpot --template $CONPOT_TEMPLATE --logfile $CONPOT_LOG --config $CONPOT_CONFIG