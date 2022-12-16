FROM amazonlinux:latest

WORKDIR /docker-rentzone-project

COPY . .

# Install dependencies
RUN yum update -y && \
    yum install -y httpd && \
    yum search wget && \
    yum install wget -y && \
    yum install unzip -y && \
    yum install php php-common php-pear -y && \
    yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y && \
    yum install git -y

# download webfiles
RUN wget https://github.com/walasee/docker-rentzone-project/raw/main/rentzone.zip

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