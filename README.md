# Docker-Apache(Manual)
Docker Container Apache Setting

## Pull apache2(httpd) package from the docker hub
docker pull httpd:2.4

##Start docker apache2(httpd) container

docker run -d -i -t -p 8080:80 -p 8081:443 --name httpd httpd:2.4

## Add Customer VirtualHost to the httpd.conf and push to the container

docker cp httpd.conf httpd:/usr/local/apache2/conf

The customer virtualHost content:
# To Load Customer VirtualHost Configuration files
IncludeOptional conf/sites/*.conf

# Docker-Apache(Manual)

