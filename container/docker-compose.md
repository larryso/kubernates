# Docker Compose
[https://docs.docker.com/compose/](https://docs.docker.com/compose/)

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

* [Docker Compose File - docker-compose-file.yml](#compose-file)

### <h2 id="compose-file"> Dockker Compose File </h2>

#### Version top-level element (optional)
Compose validates whether it can fully parse the Compose file. If some fields are unknown, typically because the Compose file was written with fields defined by a newer version of the Specification, you'll receive a warning message
