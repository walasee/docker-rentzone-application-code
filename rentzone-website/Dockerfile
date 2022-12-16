FROM amazonlinux:latest

# Install dependencies
RUN yum update -y && \
    yum install -y httpd && \
    yum search wget && \
    yum install wget -y && \
    yum install unzip -y && \
    yum install php php-common php-pear -y && \
    yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y

#install mysql5.7
RUN rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
    rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
    yum install mysql-community-server -y

# change directory
RUN cd /var/www/html

# download webfiles
RUN wget https://github.com/walasee/docker-rentzone-project/blob/main/rentzone.zip

# unzip folder
RUN unzip rentzone.zip

# copy files into html directory
RUN cp -r rentzone/* /var/www/html/

# remove unwanted folder
RUN rm -rf rentzone rentzone.zip

# exposes port 80 on the container
EXPOSE 80

# set the default application that will start when the container start
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]