# Registry proxy

To speed up the delivery process and avoid hitting external image registry limits,
CG DevX provides proxy registries for popular Docker Image Registries:

- dockerhub-proxy https://hub.docker.com
- gcr-proxy https://gcr.io
- k8s-gcr-proxy https://k8s.gcr.io
- quay-proxy https://quay.io

To start using the proxy cache, configure your docker pull commands or pod manifests to reference the proxy cache
project by adding <harbor_servername>/<proxy_project_name>/ as a prefix to the image tag. 
For example:

```shell
docker pull <harbor_server_name>/<proxy_project_name>/goharbor/harbor-core:dev
```

To pull official images or from single level repositories, make sure to include the ‘library’ namespace.

```shell
docker pull <harbor_server_name>/<proxy_project_name>/library/awesome-image:latest
```
