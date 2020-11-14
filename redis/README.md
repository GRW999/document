# redis部署

## docker部署

* docker pull redis:latest
* docker run --restart always --name redis -p 6379:6379 -d redis