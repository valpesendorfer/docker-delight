FROM ubuntu:16.04

RUN sed 's/main$/main universe/' -i /etc/apt/sources.list && \
    apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository ppa:webupd8team/java -y && \
    apt-get update && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get install -y oracle-java8-installer libxext-dev libxrender-dev libxtst-dev x11-apps libxxf86vm-dev libgfortran3 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

ADD http://step.esa.int/downloads/6.0/installers/esa-snap_sentinel_unix_6_0.sh /tmp/esasnap.sh

RUN mkdir /usr/bin/snap && bash /tmp/esasnap.sh -q -dir /usr/bin/snap
RUN rm /tmp/esasnap.sh

ENV PATH=$PATH:/usr/bin/snap/bin

RUN snap --nosplash --nogui --modules --update-all

RUN sed -i 's/# -Xmx512m/-Xmx14G/g' /usr/bin/snap/bin/gpt.vmoptions && \
    echo "-Xms1024m" >> /usr/bin/snap/bin/gpt.vmoptions

ADD launch_snap /launchers/launch_snap
ADD launch_gpt /launchers/launch_gpt

RUN chmod +x /launchers/launch_snap & chmod +x /launchers/launch_gpt

ENV DISPLAY :0

CMD /bin/bash
