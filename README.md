# hello-nginx
nginx dockerfile demo

Build the image 
# docker build -t pradhans0906/hello-world-nginx .
Run the image
# docker run -p 8080:80 pradhans0906/hello-world-nginx

deattach mode 

docker container run --publish 8080:80 -d pradhans0906/hello-world-nginx

 

