# PHP dbmaker native driver install
## download source
The following steps use Linux and php-8.1.12 as an example
```
-- create temp workspace
mkdir tmpws
-- download dbmaker-bundle package and patch to tmpws
-- download php docker image
docker pull php:cli
-- enter the docker environmen
docker run -it --rm -v$PWD/tmpws:/tmp php:cli bash
-- check php version
php -v
-- extract php source
docker-php-source extract
```

## Use pdo_odbc ext
```
cd /usr/src/php/ext/pdo_odbc/
-- patch
patch < /tmp/81.patch
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

