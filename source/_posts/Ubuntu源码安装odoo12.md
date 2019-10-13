---
title: Ubuntu源码安装odoo12
tags:
  - 笔记
date: 2019-10-13 13:50:45
---


# Ubuntu源码安装odoo12

1. **安装依赖**

   `$ sudo apt-get update`

   `$ sudo apt-get install -y git python3.5 postgresql nano virtualenv xz-utils wget fontconfig libfreetype6 libx11-6 libxext6 libxrender1 xfonts-75dpi`

2. **安装wkhtmltopdf:**

   `wget -O wkhtmltox.tar.xz \ https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz 
   $ tar xvf wkhtmltox.tar.xz
   $ sudo mv wkhtmltox/lib/* /usr/local/lib/
   $ sudo mv wkhtmltox/bin/* /usr/local/bin/
   $ sudo mv wkhtmltox/share/man/man1 /usr/local/share/man/ `

3. **构建依赖**

   `$ sudo apt-get install -y gcc python3.5-dev libxml2-dev \
   libxslt1-dev libevent-dev libsasl2-dev libssl1.0-dev libldap2-dev \
   libpq-dev libpng-dev libjpeg-dev `

4. **配置PostgreSQL**

   `$ sudo -u postgres createuser --createdb $(whoami)`

   `$ createdb $(whoami)`

5. **配置git**

   `$ git config --global user.name "Your Name"`
   `$ git config --global user.email youremail@example.com`

6. **下载源码**

   `$ mkdir ~/odoo-dev
   $ cd ~/odoo-dev
   $ git clone -b 12.0 --single-branch\ https://github.com/odoo/odoo.git
   $ cd odoo`

7. **创建一个名为odoo-12.0的虚拟环境并激活**

   `$ virtualenv -p python3 ~/odoo-12.0
   $ source ~/odoo-12.0/bin/activate`

8. **在虚拟环境中安装python的依赖包**

   `$ pip3 install -r requirements.txt`

9. **创建并开启odoo实例**

   `$ createdb odoo-test`
   `$ python3 odoo-bin -d odoo-test --addons-path=addons \` 
   `--db-filter=odoo-test$`

10. **访问http://localhost:8069登录，账号admin，密码admin** 