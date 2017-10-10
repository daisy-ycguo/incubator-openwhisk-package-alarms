# Development and Testing
## Build
Building the Alarm feed provider is a simple matter of running a `docker build` command from the root of the project. I suggest tagging the image with a memorable name, like "alarmdprovider":
``` sh
docker build -t alarmprovider .
```

## Run
Now we need to start the provider service. This is also a simple matter of running a `docker run` command, but the details are a little tricky. The service relies on a number of environment variables in order to operate properly. They are outlined below:

### Mandatory Environment Variables
|Name|Type|Description|
|---|---|---|
|DB_PROTOCOL|URL|The protocol for a persistent storage (either CouchDB or Cloudant)|
|DB_HOST|String|Host and port for your DB |
|DB_USERNAME|String|Username for your DB credentials|
|DB_PASSWORD|String|Password for your DB credentials|
|ROUTER_HOST|String|IP address of your OpenWhisk host|
|ENDPOINT_AUTH|String|The OpenWhisk auth key to use when installing the actions. Typically this would be the auth key for `whisk.system`|

An example to start the alarm feed service might look like:
```sh
docker run -d -e DB_PROTOCOL=http -e DB_HOST=172.17.0.2:5984 -e DB_USERNAME=db_admin -e DB_PASSWORD=db_password -e ROUTER_HOST=192.168.33.13 -e ENDPOINT_AUTH=1111-1111-xxxxxxxxx:yyyyyyyyyyyyyyyy -p 8090:8080 alarmprovider
```

After issuing the `docker run` command, you can confirm the service started correctly by inspecting the container with a `docker logs` command.

# Install Actions
The provided actions also need to be installed to your OpenWhisk deployment. We have automated this with a shell scripts `installCatalog.sh`

Each script requires a number of arguments which are outlined below:

|Name|Description|
|---|---|
|authKey|The OpenWhisk auth key to use when installing the actions. Typically this would be the auth key for `whisk.system`|
|edgehost|The IP address or hostname of the OpenWhisk core system.|
|dburl|The full URL (including credentials) of the CouchDB or Cloudant account used by the feed service.|
|dbprefix|A prefix to be prepended to the default DB name that will be created by the provider service. If you don't specify the prefix, you should use `undefined`.|
|apihost|The hostname or IP address of the core OpenWhisk system that will be used as the hostname for all trigger URLs. In most cases, this will be the same as `edgehost`.|

An example to install actions might look like:
```sh
./installCatalog.sh 1111-1111-xxxxxxxxx:yyyyyyyyyyyyyyyy 192.168.33.13 http://db_admin:db_password@172.17.0.2:5984 'undefined' 192.168.33.13
```
