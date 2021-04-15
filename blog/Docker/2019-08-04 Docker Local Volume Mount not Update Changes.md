# How to fix Docker (or Docker Compose) not updating local changes from  mounted volume

If you're mounting a local file or directory to a Docker container via Docker volumes or Docker Compose, it may not update if you're on Linux (and perhaps Mac too).

For example, you have a `docker-compose.yml` such as this:

```yaml
version: "3.6"

services:
  website:
    image: nginx:latest
    restart: always
    volumes:
      - ./public:/var/www/html/public
```

Where `./public` is a directory on your local machine.

You then edit `public/index.html` in your text editor or IDE, but it doesn't update `/var/www/html/public/index.html` in your Docker container.

This can be because of how the bind-mount works on Linux, it's based on inode. What's happening in the background is your text editor or IDE doesn't actually save the file you're editing directly, instead it creates a new file then copies it over the original.

You can change the default behaviour in most text editors and IDEs.

## Jetbrains

For PHPStorm, IntelliJ, Webstorm, PyCharm, etc. you can go to `Settings` > `Appearance & Behaviour` > `System Settings`, then under the heading `Syncronization` uncheck the option `Use "safe write" (save changes to a temporary file first)`.

Now edit a file, and you should see it update in your container!

## Sublime

There's an option called `atomic_save` in Sublime, so add `"atomic_save": false` and restart Sublime, and you should see it update in your container!

## Vim

Enter the command `set noswapfile` in Vim, then edit a file and you should see it update in your container!
