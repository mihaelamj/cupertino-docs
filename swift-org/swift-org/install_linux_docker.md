---
source: https://www.swift.org/install/linux/docker
crawled: 2025-11-15T21:47:20Z
---

# Linux Installation via Docker

# Linux Installation via Docker

 
 
 

 Swift official Docker images are hosted on [hub.docker.com/_/swift](https://hub.docker.com/_/swift/). Nightly builds and previews of Swift using Docker images under the [swiftlang](https://hub.docker.com/r/swiftlang/swift/tags) namespace are available in the Swift Docker repository on Docker Hub.

Swift Dockerfiles are located on [swift-docker](https://github.com/swiftlang/swift-docker) repository.

Using Docker Images 
  
 

 
- 
 Pull the Docker image from [Docker Hub](https://hub.docker.com/_/swift/):

 

```
docker pull swift

```

 
 

 
- 
 Create a container using tag `latest` and attach it to the container:

 

```
docker run --privileged --interactive --tty 
--name swift-latest swift:latest /bin/bash

```

 
 

 
- 
 Start container `swift-latest`:

 

```
docker start swift-latest

```

 
 

 
- 
 Attach to `swift-latest` container:

 

```
docker attach swift-latest

```

