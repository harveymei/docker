#
#
From centos:6.7
MAINTAINER Harvey Mei <harvey.mei@msn.com>

#
#
ADD http://mirrors.163.com/.help/CentOS6-Base-163.repo /etc/yum.repos.d/
ADD http://mirrors.zju.edu.cn/epel/6/i386/epel-release-6-8.noarch.rpm /tmp/
ADD http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm /tmp/
#COPY CentOS6-Base-163.repo /etc/yum.repos.d/
#COPY epel-release-6-8.noarch.rpm /tmp/
#COPY rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm /tmp/
RUN \
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup; \
cd /tmp/; \
yum -y install epel-release-6-8.noarch.rpm rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
#yum makecache

#
#
RUN \
yum -y install tar wget rsyslog mod_fcgid httpd httpd-devel rrdtool fping \
curl bind-utils gcc make perl perl-Net-Telnet perl-Net-DNS \
perl-LDAP perl-libwww-perl perl-RadiusPerl perl-IO-Socket-SSL \
perl-Socket6 perl-CGI-SpeedyCGI perl-FCGI perl-RRD-Simple \
perl-ExtUtils-MakeMaker

#
#
ADD http://oss.oetiker.ch/smokeping/pub/smokeping-2.6.11.tar.gz /tmp/
#COPY smokeping-2.6.11.tar.gz /tmp/
RUN \
mkdir -p /opt/smokeping/{data,var,cache}; \
cd /tmp/; \
tar xf smokeping-2.6.11.tar.gz; \
cd smokeping-2.6.11/setup/; \
./build-perl-modules.sh; \
cp -r ../thirdparty/ /opt/smokeping/; \
cd ..; \
./configure --prefix=/opt/smokeping; \
make install

#
#
ADD http://new.wedebugyou.com/static/smokeping_start_stop.txt /etc/init.d/smokeping
#COPY smokeping_start_stop.txt /etc/init.d/smokeping
RUN chmod 755 /etc/init.d/smokeping

#
#
RUN \
cd /opt/smokeping/etc/; \
chmod 600 smokeping_secrets.dist; \
cp config.dist config;

#
#
RUN \
cd /opt/smokeping/; \
ln -s /opt/smokeping/cache /opt/smokeping/htdocs/cache; \
chown -R apache cache/ data/

#
#
RUN \
cd /etc/httpd/conf.d/; \
echo "ScriptAlias /smokeping/smokeping.cgi /opt/smokeping/htdocs/smokeping.fcgi.dist" > smokeping.conf; \
echo "Alias /smokeping /opt/smokeping/htdocs" >> smokeping.conf; \
echo "" >> smokeping.conf; \
echo '<Directory "/opt/smokeping/htdocs">' >> smokeping.conf; \
echo "    Options FollowSymLinks" >> smokeping.conf; \
echo "</Directory>" >> smokeping.conf

#
#
RUN \cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

#
#
EXPOSE 80
