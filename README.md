# PGP Keyserver Site Source

This repository holds the source for the [a.sks.jda.mn](https://a.sks.jda.mn) [SKS PGP Key Server](https://sks-keyservers.net/). This is only the HTML source code for the frontend site of the PGP server, not the PGP Keyserver software used on the server itself!

If you are building your own sks-keyserver, and would like to diplay a nice frontend for it, please look at the [mattrude/pgpkeyserver-lite](https://github.com/mattrude/pgpkeyserver-lite) project for a simpler frontend site.

## Jekyll

[Jekyll](https://jekyllrb.com/) is a static site generator built in [Ruby on Rails](https://rubyonrails.org/). Jekyll is needed to build the final site, but it is not needed on the webserver necessarily. Updates can be done by a build system using this repository, then sent to the webserver.

### Installing Jekyll

Here's some steps on installation on a few different systems to build this site.

#### Installing Ruby on Ubuntu
On Ubuntu 14.04 LTS, you first need ruby installed on your setup, we will also install the development kit.

```
apt-get update; apt-get install -y git g++ ruby ruby-dev
```

Next install the needed gems and Jekyll:

```
cd pgpkeyserver
bundle install
```

Now you may use Jekyll to build the site, using the source provided in this repository.

#### Installing Ruby on Windows
First start out by downloading the current production version of the [Ruby Installer](http://rubyinstaller.org/downloads/) for windows.

##### Installing the Ruby Development Kit
After installing Ruby via the [Ruby Installer](http://rubyinstaller.org/downloads/) talked about above, you must now download the Development Kit.

1. Download the Development Kit from http://rubyinstaller.org/downloads/
1. Extract the contact into a location easy accessible to your command prompt.
1. Open a command prompt, change into the directory that you extracted the content of the Development Kit to and run the command: `rake devkit sfx=1`.

### Building the site

First change into the source directory of the site, once in, update with

```
jekyll build
```

This will build the site into the `_site` directory within the working directory, which you can serve from any webserver.

## Installing the server

### Nginx Configuration

Here's my complete configuration. Adjustments must be made as necessary to fit your config.

**/etc/nginx/sites-enabled/a.sks.jda.mn.conf:**:
```
server  {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/sks/_site;
        location / {
                return 301 https://a.sks.jda.mn$request_uri;
        }
        include /etc/nginx/snippets/pks-rewrite.conf;
        server_name a.sks.jda.mn
           # Pools we are part of (if we're up):
                pool.sks-keyservers.net *.pool.sks-keyservers.net
                *.sks.pool.globnix.net
           # Alias pools
                keys.gnupg.net http-keys.gnupg.net
                ;
        include /etc/nginx/snippets/pks.conf;
}

server  {
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        root /var/www/sks/_site;
        include /etc/nginx/snippets/pks-rewrite.conf;
        server_name a.sks.jda.mn;
        include /etc/nginx/snippets/pks.conf;

        ssl on;
        ssl_certificate /etc/letsencrypt/live/a.sks.jda.mn/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/a.sks.jda.mn/privkey.pem;
        ssl_stapling on;
}
```

**/etc/nginx/snippets/pks-rewrite.conf:**:
```
rewrite ^/stats /pks/lookup?op=stats;
rewrite ^/s/(.*) /pks/lookup?search=$1;
rewrite ^/search/(.*) /pks/lookup?search=$1;
rewrite ^/g/(.*) /pks/lookup?op=get&search=$1;
rewrite ^/get/(.*) /pks/lookup?op=get&search=$1;
rewrite ^/k/(.*) /pks/lookup?op=get&search=$1;
rewrite ^/key/(.*) /pks/lookup?op=get&search=$1;
rewrite ^/d/(.*) /pks/lookup?op=get&options=mr&search=$1;
rewrite ^/download/(.*) /pks/lookup?op=get&options=mr&search=$1;
error_page 400 /errors/400/index.html;
error_page 401 /errors/401/index.html;
error_page 403 /errors/403/index.html;
error_page 404 /errors/404/index.html;
error_page 405 /errors/405/index.html;
error_page 500 /errors/500/index.html;
error_page 501 /errors/501/index.html;
error_page 502 /errors/502/index.html;
error_page 503 /errors/503/index.html;
error_page 504 /errors/504/index.html;
```

**/etc/nginx/snippets/pks.conf:**
```
location /pks {
    access_log         off;
    proxy_pass         http://127.0.0.1:11371;
    proxy_set_header   Host            $host:$server_port;
    proxy_set_header   X-Real-IP       $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass_header  Server;
    add_header         Via "1.1 a.sks.jda.mn:11371 (nginx)" always;
    add_header         X-Robots-Tag 'nofollow, notranslate' always;
    proxy_ignore_client_abort on;
    client_max_body_size 8m;
}
```

## License

                  GNU GENERAL PUBLIC LICENSE
                    Version 3, 29 June 2007

    OpenPGP Key Server Website for keyserver.mattrude.com
    Copyright (C) 2012-2018 Matt Rude <matt@mattrude.com>

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
