---
slug: ruby-on-rails-with-nginx-on-ubuntu-9-04-jaunty
deprecated: true
description: 'Using the Ruby on Rails framework for Nginx web applications on the Ubuntu 9.04 operating system.'
keywords: ["ruby on rails", "ruby on nginx", "rails apps"]
tags: ["web applications","nginx","ubuntu","ruby"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
aliases: ['/websites/ror/ruby-on-rails-with-nginx-on-ubuntu-9-04-jaunty/','/development/ror/ruby-on-rails-with-nginx-on-ubuntu-9-04-jaunty/','/frameworks/ruby-on-rails-nginx/ubuntu-9-04-jaunty/']
modified: 2011-04-29
modified_by:
  name: Linode
published: 2009-08-19
title: 'Ruby on Rails with Nginx on Ubuntu 9.04 (Jaunty)'
relations:
    platform:
        key: ruby-on-rails-nginx
        keywords:
            - distribution: Ubuntu 9.04
authors: ["Linode"]
---



Ruby on Rails is a popular rapid development web framework that allows web designers and developers to implement fully featured dynamic web applications using the Ruby programming language. This guide describes the required process for deploying Ruby on Rails with Passenger and the nginx web server on Ubuntu 9.04 (Jaunty).

## Install Required Packages

First, make sure you have the `universe` repository enabled in your `/etc/apt/sources.list` file. Your file should resemble the following:

{{< file "/etc/apt/sources.list" >}}
## main & restricted repositories
deb http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted universe
deb-src http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted universe

deb http://security.ubuntu.com/ubuntu jaunty-security main restricted universe
deb-src http://security.ubuntu.com/ubuntu jaunty-security main restricted universe

{{< /file >}}


If you added `universe` to the items in your list, you'll need to update your repository database:

    apt-get update
    apt-get upgrade

Issue the following command to install packages required for Ruby on Rails.

    apt-get install wget build-essential ruby1.8 ruby1.8-dev irb1.8 rdoc1.8 zlib1g-dev libpcre3-dev libopenssl-ruby1.8 rubygems1.8 libopenssl-ruby libzlib-ruby libssl-dev

Create symbolic links to the installed version of Ruby:

    ln -s /usr/bin/ruby1.8 /usr/bin/ruby
    ln -s /usr/bin/irb1.8 /usr/bin/irb

Install the `rake` and `rack` gems:

    gem install rake
    gem install rack

Finally, install the version of Ruby On Rails that your application requires. Issue one the following commands for the version you need:

    gem install rails --version 2.1.2
    gem install rails --version 2.2.2
    gem install rails --version 2.3.5
    gem install rails --version 3.0.4

If you are unsure of the version you require, you can install the latest version with the following command:

    gem install rails

Additionally, the application you deploy will likely have additional dependencies. Install these dependencies before proceeding.

## Install Passenger and Nginx

Proceed to the [Phusion Passenger](http://www.modrails.com/install.html) site and locate the link for the current source code tarball. Download it as follows (substitute the link for the current version):

    cd /root
    wget http://rubyforge.org/frs/download.php/69546/passenger-2.2.11.tar.gz
    cd /opt
    tar xzvf ~/passenger-2.2.11.tar.gz

Run the Phusion Passenger installer for Nginx:

    cd passenger-2.2.11/bin
    ./passenger-install-nginx-module

You'll be greeted by the Phusion Passenger Nginx installer program. Press "Enter" to continue with the installation.

![Phusion Passenger Nginx installer program running on Ubuntu 9.04 (Jaunty).](464-01-passenger-nginx-installer.png)

When prompted for the Nginx installation method, we recommend you choose "1" to allow the installer to automatically download, compile, and install Nginx for you. Unless you have specific needs that would necessitate passing custom options to Nginx at compile time, this is the safest way to proceed.

Please do **not** remove the Passenger files from `opt` after the install. They need to stay in place or your install will not function correctly.

## Set up an Init Script for Nginx

Nginx is now installed in `/opt/nginx`, but there are no "init" scripts to control this process. Issue the following sequence of commands to download a script, move it to the proper directory, set the proper permissions and set system startup links:

    cd /opt
    wget -O init-deb.sh http://www.linode.com/docs/assets/566-init-deb.sh
    mv /opt/init-deb.sh /etc/init.d/nginx
    chmod +x /etc/init.d/nginx
    /usr/sbin/update-rc.d -f nginx defaults

You can now start, stop, and restart Nginx just like any other server daemon. For example, to start the server, issue the following command:

    /etc/init.d/nginx start

The configuration file for Nginx is located at `/opt/nginx/conf/nginx.conf`. This is the file you'll need to edit to add support for your Rails applications. A default server is already configured in this file, and it also contains examples for alternate virtual host and SSL configurations.

## Install MySQL Support (optional)

If your application uses MySQL, install the database server by following our [MySQL on Ubuntu 9.04 (Jaunty) guide](/docs/databases/mysql/ubuntu-9-04-jaunty). Once it's installed and configured properly, issue the following commands:

    apt-get install libmysqlclient15-dev libmysql-ruby
    gem install mysql --no-rdoc --no-ri -- --with-mysql-dir=/usr/bin --with-mysql-lib=/usr/lib/mysql --with-mysql-include=/usr/include/mysql

## More Information

You may wish to consult the following resources for additional information on this topic. While these are provided in the hope that they will be useful, please note that we cannot vouch for the accuracy or timeliness of externally hosted materials.

- [Ruby on Rails Home Page](http://rubyonrails.org/)
- [Ruby on Rails Documentation](http://rubyonrails.org/documentation)
- [Nginx Home Page](http://nginx.org/)
- [Nginx Documentation](http://nginx.org/en/docs/)
- [Nginx Configuration](/docs/guides/how-to-configure-nginx/)



