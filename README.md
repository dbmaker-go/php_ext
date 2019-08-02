# PHP dbmaker native driver install
## download source
The following steps use Linux and php-7.3.3 as an example
```
download php-7.3.3.tar.bz2
tar jxvf php-7.3.3.tar.bz2
```

## Use pdo_odbc ext
```
$ cd php-7.3.3/ext/pdo_odbc/
$ patch -p1 < /tmp/7x.patch
$ phpize
$ ./configure --help

# Config with standard
$ CFLAGS="-O2" ./configure --with-pdo-odbc=generic,/home/dbmaker/5.4,dmapic --with-libdir="lib/so"

# Config with bundle
$ CFLAGS="-O2" ./configure --with-pdo-odbc=generic,/opt/bundle,dmapic --with-libdir=""

$ make

# check the module directory and install
$ php-config --extension-dir
/usr/lib/php7/modules
$ cp .libs/pdo_odbc.so /usr/lib/php7/modules/
$ vi /etc/php7/php.ini
extension=pdo_odbc.so
pdo_odbc.connection_pooling=off

# check installed modules
$ php -m
```

## Use odbc ext
```
$ cd php-7.3.3/ext/odbc/
$ phpize
$ ./configure --help

# Config with standard
$ CFLAGS="-O2 -DHAVE_DBMAKER" CUSTOM_ODBC_LIBS="-ldmapic" ./configure --with-custom-odbc=shared,/home/dbmaker/5.4 --with-libdir="lib/so"

# Config with bundle
$ CFLAGS="-O2 -DHAVE_DBMAKER" CUSTOM_ODBC_LIBS="-ldmapic" ./configure --with-custom-odbc=shared,/opt/bundle --with-libdir=""

$ make

# check the module directory and install
$ php-config --extension-dir
/usr/lib/php7/modules
$ cp .libs/odbc.so /usr/lib/php7/modules/
$ vi /etc/php7/php.ini
extension=odbc.so

# check installed modules
$ php -m
```

## Windows
The Windows platform must be compiled  with the same VC version,
please see https://windows.php.net/download/

