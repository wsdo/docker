FROM ubuntu:trusty

# 道客船长荣誉出品
MAINTAINER Captain Dao <support@daocloud.io>

# APT 自动安装 PHP 相关的依赖包，如需其他依赖包在此添加
RUN apt-get update \
    && apt-get -y install \
        expect \
        curl \
        wget \
        apache2 \
        libapache2-mod-php5 \
        php5-mysql \
        php5-sqlite \
        php5-gd \
        php5-curl \
        php-pear \
        php-apc \

    # 用完包管理器后安排打扫卫生可以显著的减少镜像大小
    && apt-get clean \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \

    # 安装 Composer，此物是 PHP 用来管理依赖关系的工具
    # Laravel Symfony 等时髦的框架会依赖它
    && curl -sS https://getcomposer.org/installer \
        | php -- --install-dir=/usr/local/bin --filename=composer

# Apache 2 配置文件：/etc/apache2/apache2.conf
# 给 Apache 2 设置一个默认服务名，避免启动时给个提示让人紧张.
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# 配置默认放置 App 的目录
RUN mkdir -p /app && rm -rf /var/www/html && ln -s /app /var/www/html
COPY . /app
WORKDIR /app
RUN chmod 755 ./start.sh
# 安装 oneapm
RUN sh ./oneapm_install.sh \
    && rm ./oneapm_install.sh

EXPOSE 80
CMD ["./start.sh"]
