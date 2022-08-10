The APIMatic On-Premises Service is available as a Docker image which can be deployed using Docker for Linux or Windows. This document shows you how to setup the service for your organization. It is assumed a Linux environment is available and that the reader is familiar with running commands in a terminal.

*Using this service requires an APIMatic On-Premises License from APIMatic. For more information on this service, please contact your account manager at APIMatic or send us an email at [support@apimatic.io](mailto:support@apimatic.io).*

## Prerequisites

1. Install Docker Engine and CLI: You can see how to install this for Ubuntu at https://docs.docker.com/install/linux/docker-ce/ubuntu/.
2. Docker Hub account: You can create a free account at https://hub.docker.com/. You **must** sent your Docker Hub ID to your account manager at APIMatic to get it registered for accessing APIMatic Docker images.
3. APIMatic Code Generator On-Prem License provided by your account manager at APIMatic. 

## Installation Details

1. [Log in into the Docker Hub registry](https://docs.docker.com/engine/reference/commandline/login/) using the Docker Hub ID registered with your APIMatic account manager.
   ```bash
   docker login -u YOUR_DOCKER_HUB_ID docker.io
   ```
   This will prompt for your password.
2. Pull the APIMatic Code Generator On-prem image.
   ```bash
   docker pull apimaticio/apimatic-codegen:version-1.6.2
   ```
   This will take a few minutes, as it downloads the images from hub.docker.com.
3. Run the APIMatic Code Generator image.
   ```bash
   docker run -dit --restart always \
     -v license:/usr/app/license \
     -e LICENSEPATH=/usr/app/license \
     -p 8090:80 \
     apimaticio/apimatic-codegen:version-1.6.2
   ```
   This starts the APIMatic Code Generator and exposes it at port 8090. Notice that the license file needs to be provided by attaching a volume and its parent directory path needs to be passed in as the `LICENSEPATH` environment variable.
4. Verify that the APIMatic Code Generator service is running by calling the service API:
   ```bash
   curl localhost:8090/api/template
   ```
   This will list the SDK generators available in your license. For example: `["JAVA_ECLIPSE_JRE_LIB","CS_NET_STANDARD_LIB"]`.

## Troubleshooting

### License Issues

If the license file path is incorrect or your license file is expired, the On-Premise Service logs an error to the output and shuts down. You can checkout the Docker container's logs using:

```bash
docker container logs CONTAINER_ID
```

The CONTAINER_ID was shown when you first ran `docker run ...`. You can also retrieve it by running `docker ps` which will show you a list of all the running containers.

To resolve this:

* Check whether you are setting the `LICENSEPATH` environment variable correctly when running the Docker container. Note that `LICENSEPATH` should contain the path of the directory that contains the `License.lic` file.
* Check whether your license is expired. If your license is expired, you will see an error message in the logs about this. Please contact [support@apimatic.io](mailto:support@apimatic.io) to resolve this issue.

### macOS ZIP Files Issues

ZIP files created on macOS computers often contain additional files or hidden directories that make them problematic to decompress for the On-Premise Service. Please use a third-party compression tool that can exclude hidden directories such as `_MACOSX` during zipping.
