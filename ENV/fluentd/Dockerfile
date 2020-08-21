FROM fluent/fluentd:v1.10-1

LABEL version="1.0"
LABEL maintainer="213331@gmail.com"
LABEL description="fluentd + fluent-plugin-elasticsearch"

# Use root account to use apk
USER root

RUN apk add --no-cache --update --virtual .build-deps \
	sudo build-base ruby-dev \
	&& sudo gem install fluent-plugin-elasticsearch --no-document --version 4.0.7 \
	&& sudo gem sources --clear-all \
	&& apk del .build-deps \
	&& rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

COPY fluent.conf /fluentd/etc/

EXPOSE 24224

USER fluent
ENTRYPOINT ["fluentd"]
CMD ["-c", "/fluentd/etc/fluent.conf", "-p", "/fluentd/plugins"]
