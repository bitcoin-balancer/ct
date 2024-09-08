# Balancer Cloudflare Tunnel

Cloudflare Tunnel provides a secure and reliable way to expose Balancer's services to the Internet with a Zero Trust approach. Using the [cloudflared](https://hub.docker.com/r/cloudflare/cloudflared) image, Tunnel establishes encrypted connections between your internal services and Cloudflare's global network, eliminating the need for public IP addresses and reducing your attack surface. This allows you to control access to your applications, ensuring only authorized users and devices can reach your services.





<br/>

## Why isn't the `cloudflared` service declared directly in the `compose.yaml` file?

Traditionally, Cloudflare Tunnel has been integrated into Docker Compose by directly including the [cloudflared](https://hub.docker.com/r/cloudflare/cloudflared) service. However, this approach requires exposing the sensitive `TUNNEL_TOKEN` as an environment variable, posing a security risk to the Balancer platform.

Balancer prioritizes security by exclusively passing sensitive data to containers through filesystem-based secrets. To address this, the Balancer Cloudflare Tunnel image leverages a multi-stage build approach using [busybox](https://hub.docker.com/_/busybox). This allows the image to securely invoke [cloudflared](https://hub.docker.com/r/cloudflare/cloudflared) within a shell environment, reading the `TUNNEL_TOKEN` from a secrets file and setting it as an environment variable before execution. This ensures the token is never directly exposed within the container's environment or configuration.

For more information, visit:

- [Allow passing tunnel token via the file system](https://github.com/cloudflare/cloudflared/issues/645)

- [Passing tunnel tokens into cloudflared via Docker Compose secrets](https://gist.github.com/j0sh/b1971bfbbffeb92709cf096fb788f70c)





<br/>

## Acknowledgments

- [j0sh](https://github.com/j0sh)





<br/>

## License

[Apache v2.0](https://www.apache.org/licenses/LICENSE-2.0)