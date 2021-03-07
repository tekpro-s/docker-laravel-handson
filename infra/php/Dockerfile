#Dockerfileはテキストファイルであり、Dockerイメージを作り上げるために実行する命令をこのファイルに含めることができます。
#FROM命令はイメージビルドのためのベースイメージを設定します。
FROM php:7.4-fpm-buster
#SHELL命令は何も指定しない場合は SHELL ["/bin/sh", "-c"] がデフォルト値となります(Linuxの場合)
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

#ENV命令はコンテナ内のサーバー環境変数を設定します。
ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer

#apt-get update インストール可能なパッケージの「一覧」を更新
RUN apt-get update && \
  #Laravelのインストールに必要なパッケージをインストール
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  #パッケージインストールで使用したキャッシュファイルを削除
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  #PHPの拡張ライブラリをインストール
  docker-php-ext-install intl pdo_mysql zip bcmath

#PHPの設定ファイルをコンテナー内のファイルシステムのパス <dest> に追加します。
COPY ./php.ini /usr/local/etc/php/php.ini

#ワークディレクトリを設定
WORKDIR /work