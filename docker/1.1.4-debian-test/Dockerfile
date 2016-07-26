FROM caduvieira/plonegovbridg:1.1.4
MAINTAINER Carlos Vieira <edu.carlos.vieira@gmail.com>
LABEL Name="Identidade Digital para o Governo Brasileiro Federal para Plone - Imagem para testes"
LABEL Version="1.1.4-test"
LABEL Architecture="x86_64"

USER root

RUN testDeps="xserver-xephyr xvfb wget libxrender1 libasound2 libglib2.0-0 libdbus-glib-1-2 libgtk2.0-0 bzip2" \
 && apt-get update \
 && apt-get install -y --no-install-recommends $testDeps 


RUN wget 'https://download.mozilla.org/?product=firefox-31.0-SSL&os=linux64&lang=en-US' -O /tmp/firefox-31.0.tar.bz2 -v
RUN tar -xjvf /tmp/firefox-31.0.tar.bz2
RUN rm -rf /opt/firefox*
RUN mv firefox /opt/firefox31.0
RUN ln -sf /opt/firefox31.0/firefox /usr/bin/firefox

USER plone
COPY test.cfg /plone/instance/
RUN bin/buildout -c test.cfg -t 300

USER root

COPY xvfb /etc/init.d/
RUN chmod +x /etc/init.d/xvfb
ENV DISPLAY :99.0
RUN /etc/init.d/xvfb start

USER plone

EXPOSE 8080

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["start"]