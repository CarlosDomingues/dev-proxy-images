# dev-proxy-images

![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/CarlosDomingues/dev-proxy-images/publish-images.yaml)
![Docker Image Version](https://img.shields.io/docker/v/cfelipe/dev-proxy)

OCI-compliant images containing [Microsoft Dev Proxy](https://github.com/microsoft/dev-proxy) with pre-loaded and updated Microsoft Graph API metadata. The built images are available on [Docker Hub](https://hub.docker.com/r/cfelipe/dev-proxy).

## Usage

There are two steps to use this image:

1. Start the proxy.
2. Configure your client to route requests through the proxy.

### Running the Proxy

**Example: Using Docker**

```bash
# Short form: docker run -p 8000:8000 -p 8897:8897 -it cfelipe/dev-proxy:latest
docker run --publish 8000:8000 --publish 8897:8897 --interactive --tty cfelipe/dev-proxy:latest
```

**Example: Using Docker Compose**

Here’s an example of how to set up the dev-proxy container using a `docker-compose.yml` file:


```yaml
version: '3'
services:
  devproxy:
    image: cfelipe/dev-proxy:latest
    ports:
      - "8000:8000"
      - "8897:8897"
    stdin_open: true
    tty: true
```

To start the proxy with Docker Compose:

```bash
docker-compose up
```

### Accessing the Proxy


**Example: using `curl`:**

```bash
curl -kx http://localhost:8000 https://graph.microsoft.com/v1.0/me
```

**Example: using `python`**

```python
import requests

proxies = {
   'http': 'http://localhost:8000',
   'https': 'https://localhost:8000',
}
url = 'https://graph.microsoft.com/v1.0/me'
response = requests.get(url, proxies=proxies, verify=False)
print(response.json())
```

## Environment Variables

The proxy behavior can be configured through the following environment variables:

| Environment Variable     | Default Value                      | Description                                                                             |
|--------------------------|------------------------------------|-----------------------------------------------------------------------------------------|
| `DEVPROXY_PORT`           | `8000`                             | The port on which **dev-proxy** listens for intercepted HTTP requests.                  |
| `DEVPROXY_PRESET`         | `/devproxy/presets/m365.json`      | Path to the preset file containing Microsoft Graph API metadata.                        |
| `DEV_PROXY_LOG_LEVEL`     | `Information`                     | Log level for the proxy. Supported values are: `Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`, `None`. |


## Ports

- **8000 (Configurable)**: This is the default port where the proxy listens for HTTP requests.
- **8897**: This port is used for other internal services of the proxy (e.g., telemetry or logs).

## Useful Links

* [Microsoft Dev Proxy GitHub Repository](https://github.com/microsoft/dev-proxy)
* [Microsoft Dev Proxy Documentation](https://learn.microsoft.com/en-us/microsoft-cloud/dev/dev-proxy/overview)
* [Microsoft Graph API Documentation](https://docs.microsoft.com/en-us/graph/overview)
* [Docker Hub: cfelipe/dev-proxy](https://hub.docker.com/r/cfelipe/dev-proxy)
  
## Contributing

Contributions and suggestions are welcome! 😄

This repository contains a [.devcontainer](https://code.visualstudio.com/docs/devcontainers/containers) configuration for Visual Studio Code. If you have the [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension installed, you can open the repository in a container and start contributing right away.

[![Watch the video](https://img.youtube.com/vi/Dyweaat26nk/0.jpg)](https://www.youtube.com/watch?v=Dyweaat26nk)

To run the GitHub Actions workflow locally, you can use [act](https://github.com/nektos/act) (installed in the development container):

1. Create a `.env.variables` file with the following content:

```bash
DOCKERHUB_USERNAME=<your-dockerhub-username>
```

2. Create a `.env.secrets` file with the following content:

```bash
DOCKERHUB_PASSWORD=<your-dockerhub-pat>
```

3. Run the following command:

```bash
act --var-file .env.variables --secret-file .env.secrets
```

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.
