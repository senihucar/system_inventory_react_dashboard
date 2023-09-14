FROM  rockylinux:9.2.20230513
LABEL project="UNIX Linux System Inventory Dashboard" \
      author="Senih Ucar (IMITS UNIX)" \
      image_name="phsa-unixandlinux-dash-rockylinux9:v1.0"
ENV container docker

# see https://hub.docker.com/_/rockylinux
# RockyLinux:9 missing /usr/sbin/init -> ../lib/systemd/systemd
#  see https://github.com/rocky-linux/sig-cloud-instance-images/issues/39
RUN [ ! -f /usr/sbin/init ] && dnf -y install systemd;
RUN ([ -d /lib/systemd/system/sysinit.target.wants ] && cd /lib/systemd/system/sysinit.target.wants/ && for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum update -y
RUN echo "enabled=0" >> /etc/yum/pluginconf.d/subscription-manager.conf
RUN yum install git -y

RUN dnf module install nodejs:18/common -y
RUN npm install --global yarn
RUN yarn add node-sass@8.0.0

# Creating directory for dashboard development and set as a workingdir
RUN mkdir /root/dashboardDevelopment/
COPY ./ubold-react /root/dashboardDevelopment/
WORKDIR /root/dashboardDevelopment

# Cleaning up repositories mirrors
RUN dnf -y clean all \
    && yum clean all -y

#starts development server // need to run on background with systemd will check later
#RUN yarn
#RUN yarn start

EXPOSE 3000

VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
