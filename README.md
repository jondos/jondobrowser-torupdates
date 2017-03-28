Tor Browser Update Responses script
===================================

Dependencies
------------

The following perl modules need to be installed to run the script:
  FindBin YAML File::Slurp Digest::SHA XML::Writer

On Debian / Ubuntu you can install them with:

```
  # apt-get install libfindbin-libs-perl libyaml-perl libfile-slurp-perl \
                    libdigest-sha-perl libxml-writer-perl
```

Install Apache2
---------------
```
 $ apt-get install apache2
```

Enable mod rewrite
-----------------

edit the file /etc/apache2/apache2.conf and modify the below

  Change AllowOverride None to AllowOverride All on the <Directory /var/www/> section like below

```
  <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
  </Directory>
```

```
$ cd /var/www
$ git clone  https://github.com/lancerajee/torupdates
```

edit the file /etc/apache2/sites-available/000-default.conf  to change default directory of Apache
/var/www/torupdates/htdocs
```
$ service apache2  restart
```
```
cd htdocs
mkdir torbrowser
```

Place your final files in this folder
----------
```
cp -R /var/src/tor-browser-build/alpha/unsigned/6.5n/ torbrowser/
```
Update Response
----------
```
$ ./update_responses
```

URL Format
----------

The URL format is:
  https://something/$channel/$build_target/$tb_version/$lang?force=1

'build_target' is the OS for which the browser was built. The correspo
ndance between the build target and the OS name that we use in archive
files is defined in the config.yml file.

'tb_version' is the Tor Browser version.

'lang' is the locale.

