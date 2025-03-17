from nginx
copy nginx.conf /etc/nginx/nginx.conf
run rm -rf /usr/share/nginx/html
volume /usr/share/nginx/html
entrypoint ["nginx", "-g", "daemon off;"]
