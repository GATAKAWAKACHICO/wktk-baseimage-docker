FROM ubuntu
MAINTAINER Masaki Wakatake <masaki@w-k-t-k.com>

# iptables
# RUN service iptables stop

# update apt-get
RUN apt-get update

# install vim
RUN apt-get install -y vim vim-gtk vim-athena vim-gnome

# install git
RUN apt-get install -y git

# setup vim
ADD templates/minimal_vimrc /root/.vimrc

# vundle (vim) install
RUN git clone http://github.com/gmarik/vundle.git /root/.vim/bundle/vundle

# vim :BundleInstall
RUN vim +BundleInstall +qa

# install wget
RUN apt-get install -y wget

# install unzip
RUN apt-get install -y unzip

# install php
RUN apt-get install -y php5 php5-mysql php5-curl php5-cli php5-fpm php-pear curl imagemagick php5-imagick libapache2-mod-php5
###############################
# use php under ~/public_html #
###############################
RUN a2enmod userdir
RUN mkdir /root/public_html
# /etc/apache2/mods-enabled/php5.conf
# comment out between <IfModule mod_userdir.c> and </IfModule>
RUN service apache2 restart

# install imagemagick
RUN apt-get install -y imagemagick libmagickwand-dev

# before installing ruby
RUN apt-get install -y autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev

# Install rbenv and ruby-build
# fork from https://github.com/tcnksm/dockerfile-rbenv
RUN git clone https://github.com/sstephenson/rbenv.git /root/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build
RUN /root/.rbenv/plugins/ruby-build/install.sh
ENV PATH /root/.rbenv/bin:$PATH
RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh # or /etc/profile
RUN echo 'eval "$(rbenv init -)"' >> .bashrc

# Install multiple versions of ruby
ENV CONFIGURE_OPTS --disable-install-doc
ADD ./ruby-versions.txt /root/versions.txt
RUN xargs -L 1 rbenv install < /root/versions.txt

# Install Bundler for each version of ruby
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

# install Java
RUN apt-get install -y openjdk-7-jdk ant ant-optional

# install python
RUN apt-get install -y build-essential libsqlite3-dev libreadline6-dev libgdbm-dev zlib1g-dev libbz2-dev sqlite3 tk-dev zip libssl-dev python-dev
RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python get-pip.py
