FROM ruby:2.5.5-stretch

ENV DEBIAN_FRONTEND noninteractive
ENV PIA_VERSION 2.0.0
ENV RAILS_ENV production
ENV RAILS_SERVE_STATIC_FILES=true

RUN apt-get update \
 && apt-get install --no-install-recommends -y \
    git \
    postgresql-client \
 && apt-get clean

RUN git clone --branch $PIA_VERSION https://github.com/LINCnil/pia-back.git --depth 1 /var/www/

COPY database.yml /var/www/config/database.yml
COPY application.yml /var/www/config/application.yml

WORKDIR /var/www/

RUN gem install bundler
RUN bundle install

RUN sed -i 's/# config.force_ssl = true/config.force_ssl = true/' config/environments/production.rb

COPY entrypoint /entrypoint
RUN chmod +x /entrypoint
ENTRYPOINT ["/entrypoint"]

CMD ["bundle", "exec", "puma", "-b", "ssl://0.0.0.0:3000?key=/etc/selfsigned/development.key&cert=/etc/selfsigned/development.cert"]
