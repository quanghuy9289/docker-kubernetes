# xây dựng image mới từ image centos:latest (CENTOS 7)
FROM centos:latest

# Fix error Failed to download metadata for repo ‘AppStream’
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

# Cập nhật các gói và cài vào đó HTTPD, HTOP, VIM
RUN yum update -y
RUN yum install httpd httpd-tools -y
RUN yum install epel-release -y \
    && yum update -y \
    && yum install htop -y \
    && yum install vim -y

#Thiết lập thư mục hiện tại
WORKDIR /var/www/html
# Copy tất cả các file trong thư mục hiện tại (.) vào WORKDIR
ADD . /var/www/html

#Thiết lập khi tạo container từ image sẽ mở cổng 80
# ở mạng mà container nối vào
EXPOSE 80

# Khi chạy container tự động chạy ngay httpd
ENTRYPOINT ["/usr/sbin/httpd"]

#chạy terminate
CMD ["-D", "FOREGROUND"]