FROM amjibaly/base

WORKDIR /etc/nginx

RUN wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add -
RUN echo deb http://dl.hhvm.com/ubuntu trusty main | tee /etc/apt/sources.list.d/hhvm.list

# Install and configure nginx, disabling default site
RUN add-apt-repository -y ppa:nginx/stable \
    && apt-get update \
    && apt-get install -y \
    hhvm \
    nginx \
    && rm -rf /var/lib/apt/lists/* \
    && echo -e "\ndaemon off;" >> nginx.conf \
    && rm sites-enabled/default

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

RUN /usr/share/hhvm/install_fastcgi.sh

COPY hack.sh /etc/nginx/

CMD ["./hack.sh"]
