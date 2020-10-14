FROM    ruby

LABEL   maintainer aophion@student.21-school.ru

RUN	apt-get update -y && \
	apt-get install -y \
	curl \
	build-essential \
	libpq-dev \
	sqlite3 \
	git && \
	curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
  	curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  	echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
  	apt-get update -y && apt-get install -y nodejs yarn && \
	gem update && \
	gem install rails && \
	gem instal bundler
	

ONBUILD RUN	mkdir -p  /opt/app
ONBUILD WORKDIR		/opt/app

ONBUILD COPY app /opt/app

ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed



