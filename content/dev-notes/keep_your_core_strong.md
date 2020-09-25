+++
date = "2018-11-07"
title = "Keep Your Core Strong - Know Your BSD/Linux Tools"
slug = "keep_your_core_strong_know_your_bsd_linux_tools"
tags = ["OpenBSD", "git", "vim", "tmux"]
categories = ["dev notes"]
+++

Just as the sage advice from [The Pragmatic Programmer](https://pragprog.com/book/tpp/the-pragmatic-programmer) on mastering one text editor makes so much sense for those of us whose majority of work is rearranging bits on the digital titanic, the same holds true for knowing your operating system and investing in solid tooling for your other main productive tasks.

### Opinionated preface - Open source or GTFO

The world's resources are quickly depleting. A growing population having to work sh!tty jobs for sh!tty companies selling sh!tty stuff is not sustainable and kinda hard to keep respecting yourself as a highly paid developer building yet another duplicated piece of software on VC funds. 

If the aim is to advance humanity and avoid depleting all the resources as population explodes, then we should be looking to re-use and not create uneccessarily. Open source software that's been continually refined since the 60's/70's is continually being optimized and is able to run well on older hardware. 

We don't need another One Laptop Per Child venture creating future ewaste when we already have an abundance of capable hardware. Let's stop making batteries full of costly/harful chemicals and get more solar to continue running old laptops after their batteries have died.

### On with the show - the core utilities I invest in learning well

This page serves a purpose to me as storing minimal configuration notes. I strive to not become dependent on too much customisation for the reasons:

 - portability: the benefit of these tools are their ubiquitousness, I want the ability to grab any available device and quickly be productive without a day of setup or requiring of internet to pull down convoluted dotfiles libraries
 - learn the tool well: you may be working around the original design of the software. If it's really not there, consider contributing it to the project, or if it doesn't belong there, look for another modular program to handle that responsibility

*[some updated code in my "notfiles"](https://github.com/leonstafford/notfiles)*

#### OpenBSD

Slow is smooth and smooth is fast. Forgoing the bells, whistles and kitchen sink included in most Linux distros, OpenBSD has been planned with security and quality in mind. You're not forced to read the docs, but you'll soon realize it's the most efficient path forward. 

Amazing support for older architectures and a great welcoming community.

### Japanese support

Install relevant fcitx packages

Install  `ja-sazanami-ttf`

edit `$HOME/.config/fcitx/profile`, changing `anthy:False` to `anthy:True`

set shortcut key in `$HOME/.config/fcitx/config`

set XMODIFIERS in .xsession, along with fcitx-autostart before cwm

xset fontpath


### Vim

My minimal .vimrc

```vimscript
set ts=2 sw=2 expandtab ruler nu noswapfile colorcolumn=80                      
syntax on                                                                       
colorscheme delek                                                               
                                                                                
autocmd Filetype php setlocal expandtab tabstop=4 shiftwidth=4 softtabstop=4       
autocmd Filetype js setlocal expandtab tabstop=2 shiftwidth=2 softtabstop=2        
                                                                                
highlight ExtraWhitespace ctermbg=red guibg=red                                 
match ExtraWhitespace /\s\+$/   

" newly acquired skills
set wildmenu " show search reuslts and such as popups
set path+=** " set path to this folder and all subdirs, ie for searching

```

### git

My minimal .gitconfig

```

```

### tmux

My minimal .tmux.conf

```
# moving between panes
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# large history limit
set -g history-limit 999999999
```

### httpd

Ensure you have required files from /etc in /var/www/etc chroot (copy all when lazy)

# httpd.conf

basic example
```
prefork 10

types { include "/usr/share/misc/mime.types" }

server "localhost" {
	listen on * port 80

	directory index "index.php"

	root "/htdocs"

	location "/*.css*" {
		request no rewrite
	}

	location "/*.js*" {
		request no rewrite
	}

	location "/*.png*" {
		request no rewrite
	}

	location "/mystaticsite*" {
		directory index "index.html"
		request no rewrite
	}

	location "/*.php*" {
		fastcgi socket "/run/php-fpm.sock"
	}

	location "/posts*" {
		fastcgi socket "/run/php-fpm.sock"
	}
}
```

# PHP conf

As per [https://dev.to/nabbisen/fixing-php72fpm-on-openbsd-64-f0f](https://dev.to/nabbisen/fixing-php72fpm-on-openbsd-64-f0f), we need to manually create the missing pool file in /etc/php-fpm.d/somefile.conf

```
; Start a new pool named 'www'.
[www]

; specify user
user = www
group = www
listen.owner = www
listen.group = www
listen.mode = 0660

chroot = /var/www

listen = /var/www/run/php-fpm.sock

; allow php execution on any URL
security.limit_extensions = 

; endpoint to monitor PHP FPM
pm.status_path = /status

; define process manager
pm = dynamic
pm.max_children = 9
pm.max_spare_servers = 4
pm.min_spare_servers = 2
pm.start_servers = 3

; help pinpoint slow scripts
slowlog = /var/log/php-slow.log
request_slowlog_timeout = 2s
```

# maria

install server and client packages

run `mysql_install_db`

reload and run `mysql_secure_installation`

create a non-root user

connect via 127.0.0.1 vs localhost in WP config


# WP

# file permissions

TODO: currently 777'ing locally to allow watch script

```
find /var/www/htdocs -type d -exec chmod 775 {} \;
find /var/www/htdocs -type f -exec chmod 755 {} \;
chmod 400 /var/www/htdocs/wp-config.php
```

# watch files

watch_me_sync.sh
```
#!/bin/sh                                                                       
while :                                                                         
do                                                                              
    find $HOME/wordpress-static-html-plugin/ -type f ! -path '*/.*' |           
    entr -d  sh $HOME/deploy_changed_file.sh /_                                 
done 
```

# doas 
```
# run WP-CLI from same user as hitting from web
permit persist MYUSERNAME as www cmd wp
```

deploy_changes_file.sh
```
#!/bin/sh                                                                       
                                                                                
CHANGED_FILE=$1                                                                 
PROJECT_DIR=$HOME/wordpress-static-html-plugin/                                 
BASENAME=${CHANGED_FILE#"$PROJECT_DIR"}                                         
TARGET_PATH=/var/www/htdocs/wp-content/plugins/wordpress-static-html-plugin/${CHANGED_FILE#"$PROJECT_DIR"}
TARGET_PATH2=/var/www/htdocs/securesite.local/wp-content/plugins/wordpress-static-html-plugin/${CHANGED_FILE#"$PROJECT_DIR"}
TARGET_PATH3=/var/www/htdocs/subdirsite.local/wordpress/wp-content/plugins/wordpress-static-html-plugin/${CHANGED_FILE#"$PROJECT_DIR"}
TARGET_PATH4=/var/www/htdocs/japanesesite.local/wp-content/plugins/wordpress-static-html-plugin/${CHANGED_FILE#"$PROJECT_DIR"}
                                                                                
echo "$BASENAME"                                                                
                                                                                
cp $CHANGED_FILE $TARGET_PATH                                                   
cp $CHANGED_FILE $TARGET_PATH2                                                  
cp $CHANGED_FILE $TARGET_PATH3                                                  
cp $CHANGED_FILE $TARGET_PATH4                                                  
                                                                                
echo '' 
```

### coreutils

Refer to the excellent man pages provided by OpenBSD or take a chance with the docs from whatever other OS you're using.

My minimal shell profile (bash by default)

```
# enable viewing media offline
function dla {                                                                  
    # dl searched song to mp3                                                   
    youtube-dl --extract-audio --no-mtime --audio-format mp3 --no-playlist --default-search ytsearch -o "~/Music/%(title)s.%(ext)s" "$*" 
    if [ $? -eq 0 ]; then                                                       
      # send newest file to smplayer playlist                                   
      LATEST_FILE=$(ls -t *.mp3 | head -1)                                      
      smplayer -add-to-playlist "$LATEST_FILE"                                  
    else                                                                        
        echo "failed to download"                                               
    fi                                                                          
} 

# usage
dla some title with spaces no need to surround with quotes
```


[back](/)
