FROM ibmcom/ubuntu-ppc64le:14.04
RUN echo deb http://ftp.unicamp.br/pub/ppc64el/ubuntu/14_04/docker-ppc64el trusty main >> /etc/apt/sources.list
RUN apt-get update && \
     apt-get install --no-install-recommends -y --force-yes \
     arptables \
     bridge-utils \
     ca-certificates \
     curl \
     docker.io \
     iptables \
     libaio1 \
     python-eventlet \
     git \
     autoconf \
     gcc \
     g++ \
     libexpat1-dev \
     libaio-dev \
     libboost-all-dev \
     make \
     python-minimal
RUN curl -s https://bootstrap.pypa.io/get-pip.py > get-pip.py && python get-pip.py && rm get-pip.py
RUN pip install cattle docker-py 
RUN apt-get update && apt-get install -y --no-install-recommends libssl-dev python-dev libffi-dev gcc &&\
    pip install --upgrade requests[security]==2.7.0 &&\
    apt-get remove -y --purge python-dev libffi-dev libssl-dev gcc && apt-get autoremove -y
RUN curl -s jq > /usr/bin/jq; chmod +x /usr/bin/jq
#RUN curl -s -L https://get.docker.com/builds/Linux/x86_64/docker-1.6.0 > /usr/bin/docker; chmod +x /usr/bin/docker
#RUN curl -s -L https://github.com/rancherio/thin-provisioning-tools/releases/download/rancher-v0.1/pdata_tools > /usr/bin/pdata_tools; chmod +x /usr/bin/pdata_tools
### @TODO: MOVE THIS UP LATER ###
RUN sudo apt-get install -y git autoconf gcc g++ libexpat1-dev libaio-dev libboost-all-dev make
RUN git clone --no-checkout https://github.com/rancher/thin-provisioning-tools.git && cd thin-provisioning-tools && git fetch && git checkout rancher-v0.1
RUN curl -o thin-provisioning-tools/autoconf/config.sub 'http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD'
RUN curl -o thin-provisioning-tools/autoconf/config.guess 'http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD'
RUN cd thin-provisioning-tools && autoconf && ./configure && make && make install
RUN rm -rf thin-provisioning-tools
RUN apt-get update && apt-get install -y --no-install-recommends conntrack
RUN mkdir -p /var/lib/cattle /var/lib/rancher
COPY register.py resolve_url.py agent.sh run.sh /
ENTRYPOINT ["/run.sh"]
LABEL "io.rancher.container.system"="rancher-agent"
ENV RANCHER_AGENT_IMAGE rancher/agent:v0.8.2
