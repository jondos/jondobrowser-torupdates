JonDoBrowser Update Responses script
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
$ git clone https://github.com/jondos/jondobrowser-torupdates.git
```

Edit the file /etc/apache2/sites-available/000-default.conf  to change default directory of Apache
/var/www/torupdates/htdocs

```
 DocumentRoot /var/www/torupdates/htdocs
```

Restart Apache
```
$ service apache2  restart
```
```
cd htdocs
mkdir releases
```

Place your final files in this folder
----------
alpha
```
cp -R /absolute/path/to/jondobrowser-build/alpha/unsigned/7.5a9.1.1/ releases/
```
product
```
cp -R /absolute/path/to/jondobrowser-build/product/unsigned/7.5.9.11/ releases/
```
Update version information in config.yml
----------

Please refer to this wiki page.
https://github.com/jondos/jondobrowser-build/wiki/Releasing-a-new-product-build#update-download-server-configuration

Update mar-tools
----------
For linux x64 machine, copy mar-tools-linux64.zip from latest build directory to mar-tools directory.
```
$ cp -f releases/7.5.9.11/mar-tools-linux64.zip mar-tools/
```
Generate incremental update (.mar) files
----------
```
$ ./gen_incrementals
```
Sign mar files
----------

Please refer to this wiki page.
https://github.com/jondos/jondobrowser-misc/tree/master/codesign-jondo

Update Response
----------
```
$ ./update_responses
```
Create release symlink files (for product only)
----------
```
$ mkdir -p htdocs/releases/current
$ ./create_release_links --from-version 7.5.9.11 --to-directory current --basedir /var/www/torupdates/htdocs/releases
```

URL Format
----------

The URL format is:
  https://something/$channel/$build_target/$tb_version/$lang?force=1

'build_target' is the OS for which the browser was built. The correspo
ndance between the build target and the OS name that we use in archive
files is defined in the config.yml file.

'tb_version' is the JonDoBrowser version.

'lang' is the locale.

