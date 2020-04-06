FROM ruby

ONBUILD RUN	curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
            echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
            apt-get update && \
            apt-get install -y	yarn \
                                build-essential \
                                libpq-dev \
                                nodejs sqlite3 && \
            yarn install --check-files && \
            yarn upgrade && \
            gem install rails

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE 3000

ONBUILD RUN bundle update --bundler && \
            bundle install

ONBUILD RUN yarn install --check-files

ONBUILD RUN bundle exec rake db:migrate
ONBUILD RUN bundle exec rake db:seed
