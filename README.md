# nagios


sudo su
yum install httpd php
yum install gcc glibc glibc-common
yum install gd gd-devel

adduser -m nagios
passwd nagios

groupadd nagioscmd
usermod -a -G nagioscmd nagios
usermod -a -G nagioscmd apache

mkdir ~/downloads
cd ~/downloads

wget https://sourceforge.net/projects/nagios/files/nagios-4.x/nagios-4.0.8/nagios-4.0.8.tar.gz/download?use_mirror=udomain
wget https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGx2Tm55anBZMndsWVZnT1F6NVkwY0FVX05zZ3xBQ3Jtc0tuVUFLQmJhMXpUZXg3QU16M0todl9uZGRpTS14UTlJTzhuT0dwLXpYVTRYdFlvQUlTZU5PS3F3RHhOYzd1OUxValgwMWpWTDhpU1FmbWh2M3hSSFBRVU1FSHBWLWhuTHZkYXAtOGZ1Z3d0a3YwMDZKNA&q=http%3A%2F%2Fnagios-plugins.org%2Fdownload%2Fnagios-plugins-2.0.3.tar.gz

tar zxvf nagios-4.0.8.tar.gz
cd nagios-4.0.8

./configure --with-command-group=nagioscmd
make all


make install
make install-init
make install-config
make install-commandmode


make install-webconf

htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
service httpd restart


cd ~/downloads
tar zxvf nagios-plugins-2.0.3.tar.gz
cd nagios-plugins-2.0.3

./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
make install


chkconfig --add nagios
chkconfig nagios on

/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

service nagios start
service httpd restart
