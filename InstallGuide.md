# Development and Testing
## Build
Building the Alarm feed provider is a simple matter of running a `docker build` command from the root of the project. I suggest tagging the image with a memorable name, like "kafkafeedprovider":
docker build -t alarmprovider .

docker run -d -e DB_PROTOCOL=http -e DB_HOST=172.17.0.2:5984 -e DB_USERNAME=whisk_admin -e DB_PASSWORD=some_passw0rd -e ROUTER_HOST=192.168.33.13 -e ENDPOINT_AUTH=789c46b1-71f6-4ed5-8c54-816aa4f8c502:abczO3xZCLrMN6v2BKK1dXYFpXlPkccOFqm12CdAsMgRU4VrNZ9lyGVCGuMDGIwP -p 8090:8080 alarmprovider

./installCatalog.sh 789c46b1-71f6-4ed5-8c54-816aa4f8c502:abczO3xZCLrMN6v2BKK1dXYFpXlPkccOFqm12CdAsMgRU4VrNZ9lyGVCGuMDGIwP 192.168.33.13 http://whisk_admin:some_passw0rd@172.17.0.2:5984 'undefined' 192.168.33.13

