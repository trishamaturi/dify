FROM langgenius/base:1.0.0-bullseye-slim as langgenius-api

LABEL maintainer="takatost@gmail.com"

ENV FLASK_APP app.py
ENV EDITION SELF_HOSTED
ENV DEPLOY_ENV PRODUCTION
ENV CONSOLE_API_URL http://127.0.0.1:5001
ENV CONSOLE_WEB_URL http://127.0.0.1:3000
ENV SERVICE_API_URL http://127.0.0.1:5001
ENV APP_API_URL http://127.0.0.1:5001
ENV APP_WEB_URL http://127.0.0.1:3000

EXPOSE 5001

WORKDIR /app/api

COPY requirements.txt /app/api/requirements.txt

RUN apt-get install -y libssl-dev wget && \
    wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz && \
    tar -zxvf mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz && \
    cp -r mysql-5.7.35-linux-glibc2.12-x86_64/include/* /usr/include/ && \
    cp -r mysql-5.7.35-linux-glibc2.12-x86_64/lib/* /usr/lib/x86_64-linux-gnu/ && \
    rm -rf mysql-5.7.35-linux-glibc2.12-x86_64 mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz

ENV MYSQLCLIENT_CFLAGS="-I/usr/include/mysql"
ENV MYSQLCLIENT_LDFLAGS="-L/usr/lib/x86_64-linux-gnu"

# ENV MYSQLCLIENT_CFLAGS="-I/opt/homebrew/Cellar/mysql/8.0.33_3/include/mysql"
# ENV MYSQLCLIENT_LDFLAGS="-L/opt/homebrew/Cellar/mysql/8.0.33_3/lib"

RUN pip install -r requirements.txt

COPY . /app/api/

COPY docker/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ARG COMMIT_SHA
ENV COMMIT_SHA ${COMMIT_SHA}

ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]