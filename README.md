# PHP dbmaker native driver install

## PHP security update

Use a currently supported, patched PHP release. PHP 8.2.35 fixes CVE-2026-109618 and CVE-2026-109619; earlier PHP 8.2 images must not be used for web workloads. This repository's `82.patch` is validated against PHP 8.2.35.

## Download source

The following steps use Linux and PHP 8.2.35 as an example.
```
-- create temp workspace
mkdir tmpws
-- download dbmaker-bundle package and patch to tmpws
-- download php docker image
docker pull php:8.2.35-cli
-- enter the docker environmen
docker run -it --rm -v "$PWD/tmpws:/tmp" php:8.2.35-cli bash
-- check php version
php -v
-- extract php source
docker-php-source extract
```

## Use pdo_odbc ext
```
cd /usr/src/php/ext
-- patch
patch < /tmp/82.patch
-- config
docker-php-ext-configure pdo_odbc \
  --with-pdo-odbc=generic,/opt/bundle,dmapic \
  --with-libdir=""
make
cp .libs/pdo_odbc.so /tmp
```

## Use odbc ext
```
cd /usr/src/php/ext/odbc/
-- patch
sed -i '1iAC_DEFUN([PHP_ALWAYS_SHARED],[])dnl' config.m4
-- config
CUSTOM_ODBC_LIBS="-ldmapic" \
docker-php-ext-configure odbc \
  --with-custom-odbc=shared,/opt/bundle \
  --with-libdir=""
make
cp .libs/odbc.so /tmp
```

## Windows
The Windows platform must be compiled  with the same VC version,
please see https://windows.php.net/download/

