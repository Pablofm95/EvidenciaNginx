# EvidenciaNginx
Explicación de los Server Blocks de Nginx

server {                
	listen 443 ssl http2;
	listen       [::]:443 ssl http2;

        server_name  pablofonta.es;
        root         /var/www/html/Portfolio;

	include /etc/nginx/common/ssl.conf;	 
    	
        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
	#try_files $uri $uri/ =404;
	try_files $uri $uri/ /index.php$is_args$args;
        }

        error_page 404 /errorPortfolio.html;
        location = /errorPortfolio.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
	
	 location ~ \.php$ {
      try_files $uri =404;
      fastcgi_pass unix:/var/opt/remi/php80/run/php-fpm/php-fpm.sock;
      fastcgi_index index.php;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      include fastcgi_params;
      }
    }


server {
        listen       80;
        listen       [::]:80;                
        server_name _;
	return 301 https://$host$request_uri;

    }

server {
        listen       443;
        listen       [::]:443;
        server_name  tienda.pablofonta.es;
        #root         /var/www/html/wordpress;
        #index index.php  index.html index.htm;

	 include /etc/nginx/common/ssl.conf;
        error_page 404 /404.html;
        location = /404.html {
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location / {
                proxy_pass http://127.0.0.1:8080;
                                                               
        }
    }

server {
        listen       443;
        listen       [::]:443;
        server_name  node.pablofonta.es;
        
 	include /etc/nginx/common/ssl.conf;
        error_page 404 /404.html;
        location = /404.html {
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location / {
                proxy_pass http://127.0.0.1:5000;

        }
    }

server {
        listen       443;
        listen       [::]:443;
        server_name  appserver.pablofonta.es;

        include /etc/nginx/common/ssl.conf;
        error_page 404 /404.html;
        location = /404.html {
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location / {
               proxy_pass http://13.59.125.76:80;
	
        }
    }
