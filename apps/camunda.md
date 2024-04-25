---
layout: default
title: Camunda
has_children: false
parent: Self Hosted Apps
---

# Camunda

## Useful links

- [How to deploy a diagram](https://docs.camunda.io/docs/self-managed/modeler/desktop-modeler/deploy-to-self-managed/)
- [You can build forms with the modeler](https://docs.camunda.io/docs/guides/utilizing-forms/)
- [You can download the web modeler here](https://camunda.com/download/modeler/)
- [There are lots of connectors: here](https://docs.camunda.io/docs/components/connectors/out-of-the-box-connectors/available-connectors-overview/?ootb=outbound)


You can connect forms to user tasks in the modeler. If you click on the wrench of a user task,
a properties pane will show up on the right. Select "Camunda Form (linked)" for the form type.
And put the form id in the input box. For example "form_1".

## Deployment

Use the docker-compose.yaml that the project provides. Put Caddy in front of it
to access all the different services.

## Reverse proxy

```yaml
version: "3.7"
services:
  caddy:
    image: caddy:latest
    ports:
      - 80:80
      - 443:443
    networks:
      - camunda-platform_camunda-platform
    volumes:
      - /etc/ssl/certs/star.company.com.pem:/config/cert.pem
      - /etc/ssl/private/star.company.com.key:/config/cert.key
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_data:/data
    restart: unless-stopped

networks:
  camunda-platform_camunda-platform:
    external: true
```

## Caddyfile

```yaml
tasklist.company.com {
  reverse_proxy tasklist:8080
  tls /config/cert.pem /config/cert.key
}

operate.company.com {
  reverse_proxy operate:8080
  tls /config/cert.pem /config/cert.key
}

camunda.company.com {
  reverse_proxy h2c://zeebe:26500
  tls /config/cert.pem /config/cert.key
}

connectors.company.com {
  reverse_proxy connectors:8080
  tls /config/cert.pem /config/cert.key
}
```
