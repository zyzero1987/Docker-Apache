# Docker-Apache
Once you have all setting done, then you can find the following results  
Image build successful  
![Alt text](img/1.JPG?raw=true "Title")
Image runs successful  
![Alt text](img/2.JPG?raw=true "Title")

## Docker Container Apache Manual Setting
1. Pull apache2(httpd) package from the docker hub
```
docker pull httpd:2.4
```
2. Start docker apache2(httpd) container
```
docker run -d -i -t -p 8080:80 -p 8081:443 --name httpd httpd:2.4
```
3. Add apache2 to load extra conf files from a custom location  
If you want the container apache2 service that can load extra configuration files, please add the following line to the end of httpd.conf.  
```
IncludeOptional conf/sites/*.conf
```
4. If you want to use my, you can just push the file that located at the conf file to the container
```
docker cp conf/httpd.conf httpd:/usr/local/apache2/conf
```
## Docker Container Apache Dockerfile Setting

1. Create Dockerfile with the following content
```
FROM httpd:2.4

#Create a proxy local sites folder, a location to have proxypass #setting
RUN mkdir -p /usr/local/apache2/conf/sites

#Copy enable and load httpd conf files that it locate conf/sites
COPY conf/httpd.conf /usr/local/apache2/conf/httpd.conf

EXPOSE 80

CMD ["httpd-foreground"]
```

2. Build a new httpd docker image
```
docker build -t local-httpd:1.0 .
```
3. Start the new httpd image
```
docker run -d -i -t -p 8080:80 -p 8081:443 --name httpd local-httpd:1.0
```
