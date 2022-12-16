
# specify base image 
FROM amazonlinux

# install dependencies

#update linux server 
RUN sudo yum update -y

# install apache 
RUN sudo yum install -y httpd httpd-tools mod_ssl
    SUDO systemctl enable httpd 
    sudo systemctl start httpd

#3. install php 7.4
RUN sudo amazon-linux-extras enable php7.4
    sudo yum clean metadata
    sudo yum install php php-common php-pear -y
    sudo yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y

#4. install mysql5.7
RUN sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
    sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
    sudo yum install mysql-community-server -y
    sudo systemctl enable mysqld
    sudo systemctl start mysqld

#5. download the RentZone zip from s3 to the html directory on the ec2 instance
RUN sudo aws s3 sync s3://aosnote-rentzone-web-files /var/www/html

#6. unzip the RentZone zip folder
RUN cd /var/www/html
    sudo unzip rentzone.zip

#7. move all the files and folder from the RentZone directory to the html directory
RUN sudo mv rentzone/* /var/www/html

#8. move all the hidden files from the RentZone directory to the html directory
RUN sudo mv rentzone/.well-known /var/www/html
    sudo mv rentzone/.env /var/www/html
    sudo mv rentzone/.htaccess /var/www/html

#9. delete the RentZone and RentZone.zip folder
RUN sudo rm -rf rentzone rentzone.zip


#10. enable mod_rewrite on ec2 linux
RUN sudo sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf


#11. set permissions
RUN sudo chmod -R 777 /var/www/html
    sudo chmod -R 777 storage/


#13 restart server
RUN sudo service httpd restart

#14 exposes port 80 on the container
EXPOSE 80

#15 set the default application that will start when the container start
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]