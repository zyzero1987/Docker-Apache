# Docker-Apache

Once you have all setting done, then you can find the following results  
1. Build image  successful  
![Alt text](img/1.JPG?raw=true)

2. Run image successful  
![Alt text](img/2.JPG?raw=true)

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
## Docker Apache Volume (Windows)
On windows OS, you need to use the application path of Kitematic to setup a container volume  
Windows OS application Kitematic default document path is 
```
C:\Users\(your login user name)\Documents\Kitematic
```

Docker windows volume base path  
![Alt text](img/3.JPG?raw=true)

Where to see your volume via the application  
![Alt text](img/4.JPG?raw=true)

### Volume setup
For a container, setting the volume mount path should be follow the -v and for windows user, just keep in mind, use **//c/** instead of **C:/**  
```
docker run -d -i -t -p 8080:80 -p 8081:443 -v //c/Users/(your login user name)/Documents/Kitematic:/usr/local/apache2/conf/sites --name httpd local-httpd:1.0
```

### Enable Volume Setting
After you have the command run, get back to the application Kitematic panel to enable the volume  
![Alt text](img/5.JPG?raw=true)

### Change Volume path
If you want to change the path, just keep in mind, you need to keep the base path(//c/Users/(your login user name)/Documents/Kitematic) as prefix.  
If you dont, the applicatioln wont let you do it  
![Alt text](img/6.JPG?raw=true)
