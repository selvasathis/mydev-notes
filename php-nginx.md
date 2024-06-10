## PHP file expose by using nginx

### nginx install 
    sudo apt-get update -y
    sudo apt-get install nginx -y


###  install the PHP-FPM
    sudo apt install php-fpm

### Configure the configuration file /etc/nginx/sites-available/default
```json
server {
    #
            server_name staff.vsquare.guru;
    #
    #       root /var/www/example.com;
    #       index index.html;
    #
            location / {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_redirect off;
                proxy_http_version 1.1;
                proxy_pass      http://localhost:3001;
    #               try_files $uri $uri/ =404;
            }
    
    
        listen [::]:443 ssl ipv6only=on; # managed by Certbot
        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/staff.vsquare.guru/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/staff.vsquare.guru/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    
    }
    server {
        if ($host = staff.vsquare.guru) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
    
    
            listen 80;
            listen [::]:80;
            server_name staff.vsquare.guru;
        return 404; # managed by Certbot
    
    
    }
server {
        listen 80 default_server;
        listen [::]:80 default_server;
    
        root /var/www/html;
        index index.html index.php index.htm index.nginx-debian.html;
    
        server_name _;
    
        location phpm/ {
            try_files $uri $uri/ =404;
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
    
        location ~ /\.ht {
            deny all;
        }
    }
```

### edit the sinppet configuration on /etc/nginx/snippets/fastcgi-php.conf
### command the try_files $fastcgi_script_name =404;

    # regex to split $uri to $fastcgi_script_name and $fastcgi_path
    fastcgi_split_path_info ^(.+?\.php)(/.*)$;

    # Check that the PHP script exists before passing it
    #try_files $fastcgi_script_name =404;

    # Bypass the fact that try_files resets $fastcgi_path_info
    # see: http://trac.nginx.org/nginx/ticket/321
    set $path_info $fastcgi_path_info;
    fastcgi_param PATH_INFO $path_info;

    fastcgi_index index.php;
    include fastcgi.conf;

### check the syndex
    sudo nginx -t

### reststart nginx
    sudo systemctl restart nginx