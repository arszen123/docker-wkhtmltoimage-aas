# docker-wkhtmltopdf-aas

wkhtmltoimage in a docker container as a web service.

This image is based on the 
[wkhtmltopdf container](https://hub.docker.com/r/openlabs/docker-wkhtmltopdf-aas).

## Running the service

Run the container with docker run and binding the ports to the host.
The web service is exposed on port 80 in the container.

```sh
docker run -d -P arszen123/docker-wkhtmltoimage-aas
```

The container now runs as a daemon.

Find the port that the container is bound to:

```sh
docker port 071599a1373e 80
```

where 071599a1373e is the container SHA that docker assigned when
`docker run` was executed in the previous command.

Take a note of the public port number where docker binds to.

## Using the webservice

There are multiple ways to generate a Image of HTML using the
service.

### Uploading a HTML file

This is a convenient way to use the service from command line
utilities like curl.

```sh
curl -X POST -vv -F 'file=@path/to/local/file.html' http://<docker-host>:<port>/ -o path/to/output/file.jpeg
```

where:

* docker-host is the hostname or address of the docker host running the container
* port is the public port to which the container is bound to.

### JSON API

If you are planning on using this service in your application,
it might be more convenient to use the JSON API that the service
uses.

Here is an example using python requests:

```python
import json
import requests

url = 'http://<docker_host>:<port>/'
data = {
    'contents': open('/file/to/convert.html').read().encode('base64'),
}
headers = {
    'Content-Type': 'application/json',    # This is important
}
response = requests.post(url, data=json.dumps(data), headers=headers)

# Save the response contents to a file
with open('/path/to/local/file.jpeg', 'wb') as f:
    f.write(response.content)
```