FROM nginx

RUN apt-get update && \
    apt-get install -y gcc && \ 
    apt-get install -y libfcgi-dev && \
    apt-get install -y spawn-fcgi

COPY ../server/hello_server.c /.
COPY ../server/nginx.conf /etc/nginx/.
COPY ../server/entry.sh /.

RUN gcc hello_server.c -lfcgi
RUN chmod +x entry.sh

ENTRYPOINT ["/entry.sh"]

