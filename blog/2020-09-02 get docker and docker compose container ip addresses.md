# Get the IP addresses of Docker containers


To list all Docker containers and their corresponding IP addresses, run:
```bash
docker inspect -f "{{.Name}}: {{.NetworkSettings.IPAddress }}" $(docker ps -aq)
```

To get the IP address for a specific Docker container, run:

```bash
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" CONTAINER_ID
```

(Where CONTAINER_ID is the container name / ID.)


To list all Docker Compose containers and their corresponding IP addresses, run:
```bash
docker inspect -f "{{.Name}}: {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" $(docker ps -aq) | cut -c2-
```

You can add these to you `~/.profile`, `~/.bashrc`, or `~/.zshrc` in handy aliases like:

For Docker:
```bash
# Get IP addresses of all Docker containers
alias dips='docker inspect -f "{{.Name}}: {{.NetworkSettings.IPAddress }}" $(docker ps -aq)'

# Get IP addess of specific Docker container. Usage: dip CONTAINER_NAME_OR_ID
alias dip='docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"'
```

For Docker Compose:
```bash
# Get IP addresses of all Docker Compose containers
alias dcips='docker inspect -f "{{.Name}}: {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" $(docker ps -aq) | cut -c2-'
```
