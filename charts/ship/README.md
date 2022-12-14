# Ship

[![Image Size](https://img.shields.io/docker/image-size/glenndehaan/ship)](https://hub.docker.com/r/glenndehaan/ship)

![Ship Service Overview Page](https://user-images.githubusercontent.com/7496187/176439619-eb5e8cf1-b0f8-481c-8ec1-bb0ae7a5a45b.png)

## What is it?
Ship is a simple stack/service manager for Kubernetes/Docker Swarm.
It allows for example: Development teams to deploy new image version to a Kubernetes or Docker Swarm Cluster.
Developers are then also able to see the logs for either the deployment/service or connected pods/tasks for easy debugging.

It also provides an option to scale deployments/services and force re-deploy deployments/services.

## Development Usage
Make sure you have Node.JS 16.x installed then run the following commands in your terminal:
```text
npm ci
npm run tailwind
npm run dev
```

> Please note that not all functions are available when running locally. Some functions are only available on a docker swarm instance when running in swarm mode

## Run ship in production
### Kubernetes
Ship is available for your own kubernetes cluster.
Follow the guide below to install the app onto your own cluster:

> We recommend you to use an ingress controller like Traefik or Nginx in front of Ship

> Please note that right now Ship does not have built-in user management! So don't open up Ship to the outside world unless you are using for example an IP Whitelist

[Helm](https://helm.sh) must be installed to use the ship chart.
Please refer to Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```shell
helm repo add glenndehaan https://glenndehaan.github.io/charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve the latest versions of the packages.
You can then run `helm search repo glenndehaan` to see the charts.

To install the ship chart:
```shell
helm install ship glenndehaan/ship
```

You can refer to the `values.yaml` to customize the deployment:
https://github.com/glenndehaan/charts/blob/master/charts/ship/values.yaml

### Docker Swarm
Ship is available for your own docker swarm cluster.
Follow the guide below to install the app onto your own cluster:

> We recommend you to use a reverse proxy like Traefik or Nginx in front of Ship

> Please note that right now Ship does not have built-in user management! So don't open up Ship to the outside world unless you are using for example an IP Whitelist

* Create a ship-stack.yml file with the following contents:
```yaml
version: '3.8'

services:
  ship:
    image: glenndehaan/ship:latest
    deploy:
      labels:
        # Example headers for the traefik v2 reverse proxy
        - traefik.enable=true
        - traefik.http.routers.ship.entrypoints=websecure
        - traefik.http.routers.ship.rule=Host(`ship.example.com`)
        - traefik.http.routers.ship.tls=true
        - traefik.http.services.ship.loadbalancer.server.port=3000
      placement:
        constraints:
          - node.role == manager
    environment:
      # Can be used to hide specific services from the ship overview page
      HIDDEN_SERVICES: 'ship_ship,ship_ship-agent'
      # Defines the maximum allowed scale. This means you can't scale a service with more containers then this amount
      MAX_SCALE: '20'
      # Can be used to instruct Ship to use an SSO providers username header
      # AUTH_HEADER: ''
      # Can be used to enable custom webhooks to forward Ship events to other services
      # CUSTOM_WEBHOOK: 'https://webhook1.example.com/hook,https://webhook2.example.com/hook'
      # Can be used to enable slack notifications whenever an action on ship is performed
      # SLACK_WEBHOOK: ''
      # Can be used to enable email notifications whenever an action on ship is performed
      # EMAIL_SMTP_HOST: 'smtp.example.com'
      # EMAIL_SMTP_PORT: '25'
      # EMAIL_SMTP_SECURE: 'false'
      # EMAIL_FROM: 'noreply@example.com'
      # EMAIL_TO: 'user1@example.com,user2@example.com'
      # Can be used to disable service modifications during specific days/times
      # LOCKOUT_EXCEPTIONS: 'user@example.com' # Specifies usernames who can bypass the lockout rules
      # LOCKOUT_SERVICE_REGEX: '.*-live' # Specifies which services are affected by the lockout rules
      # LOCKOUT_DAYS: '0,5,6' # Disallow specific days: 0 = Sunday, 1 = Monday, 2 = Tuesday, 3 = Wednesday, 4 = Thursday, 5 = Friday, 6 = Saturday
      # LOCKOUT_AFTER_HOUR: '' # Disallow modifications after a specified hour, note: we are using the UTC timezone
      # Can be enabled to include debug data as JS window parameters
      # DEBUG_DOCKER: 'true'
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /opt/ship_data:/data
  ship-agent:
    image: glenndehaan/ship-agent:latest
    deploy:
      mode: global
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

* Run `docker stack deploy -c ship-stack.yml ship` this pulls ship and starts it on the docker swarm

## Custom Webhooks
Ship can forward events to other services through the help of webhooks (POST Request).
Below you will find an example payload of what you can expect:
```json
{
  "type": "force_update",
  "username": "Anonymous",
  "service": "test_test",
  "params": {},
  "time": 1651821585213
}
```

Below are some options that get send with different events:
```text
{
  "type": "attempt_update | update | attempt_force_update | force_update | attempt_scale | scale | attempt_restore | restore",
  "username": "Anonymous | Authenticated User Email/Username",
  "service": "stack_service",
  "params": {
    image: "library/repo", // Only gets send with type: update
    old_image_version: "latest | tag", // Only gets send with type: update
    new_image_version: "latest | tag" // Only gets send with type: update
  },
  "time": 0
}
```

## Screenshots (Docker Swarm Mode)

### Nodes Page
![Nodes Page](https://user-images.githubusercontent.com/7496187/176439623-ce246c1a-f7ed-4b4b-95a0-5739c6ab8550.png)

### Allocation Page
![Allocation Page](https://user-images.githubusercontent.com/7496187/176439234-fec7be25-b6ea-47f1-819c-0b20c23a523a.png)

### Usage Page
![Usage Page](https://user-images.githubusercontent.com/7496187/176439081-22d9d917-0ac8-4e44-b220-55d7a6e7f437.png)

### Service Detail Page
![Service Detail Page](https://user-images.githubusercontent.com/7496187/174819447-d0e8ad9b-8242-4e2d-b423-65aa2eb938f8.png)

### Service Logs
![Service Logs](https://user-images.githubusercontent.com/7496187/174820206-ec087cf3-bf93-4a7e-ad06-24501354b692.png)

### Task Detail Page
![Task Detail Page](https://user-images.githubusercontent.com/7496187/174819430-220d3245-baab-40be-92d3-bc9d9ecb7f64.png)

### Update the service image
![Update Service](https://user-images.githubusercontent.com/7496187/174246476-201846b8-4d5e-4980-b220-fb70c3ac3d2f.png)

### Force Update/Re-deploy Service
![Force Update Service](https://user-images.githubusercontent.com/7496187/174246472-90461d5a-820d-461d-ad87-41fa867e1eae.png)

### Scale Service
![Scale Service](https://user-images.githubusercontent.com/7496187/174246469-565396e4-781e-4853-85b7-aa0ef3dc88b0.png)

### Restore Service
![Restore Service](https://user-images.githubusercontent.com/7496187/174246466-179b1a9a-0c97-4727-b4e1-a9a389e7a268.png)

## Screenshots (Kubernetes Mode)

### Deployment Detail Page
![Deployment Detail Page](https://user-images.githubusercontent.com/7496187/193122188-d88538b9-aca4-4ba1-af21-fd1a7b9cc782.png)

### Pod Detail Page
![Pod Detail Page](https://user-images.githubusercontent.com/7496187/193122182-cc5b621d-ce7a-4104-87b8-c9d5cc778abd.png)

## Screenshots (Notifications)

### Email Notification
![Email Notification](https://user-images.githubusercontent.com/7496187/166509782-187f44da-8dde-4dfd-8d54-53f4b0b0f049.png)

### Slack Notification
![Slack Notification](https://user-images.githubusercontent.com/7496187/168585953-d55d478b-1c29-4907-b7eb-436efa52214c.png)

## License

MIT
