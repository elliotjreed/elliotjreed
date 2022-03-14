# Symfony logging with Rollbar

The Symfony Rollbar bundle doesn't support Symfony 6. Attempting to use the built-in Monolog Rollbar handler in Symfony directly results in the following:

```bash
Uncaught Error: Class "RollbarNotifier" not found
```

Fortunately we can use the `Monolog\Handler\RollbarHandler` still if we define it as a custom service in `services.yaml` and pass in `Rollbar\RollbarLogger` and the required arguments.

Fire require the Rollbar package:

```bash
composer require rollbar/rollbar
```

Then in `services.yaml` add the following:

```yaml
Rollbar\RollbarLogger:
  class: Rollbar\RollbarLogger
  arguments:
    - {
      access_token: '%env(ROLLBAR_ACCESS_TOKEN)%',
      environment: 'production'
    }

Monolog\Handler\RollbarHandler:
  class: Monolog\Handler\RollbarHandler
```

Then in `monolog.yaml` in either `packages/prod/monolog.yaml` or under `when@prod` in `packages/monolog.yaml` add the following:

```yaml
  monolog:
    handlers:
      main:
        type: fingers_crossed
        action_level: warning
        handler: rollbar
        excluded_http_codes: [ 405, 404 ]
        buffer_size: 50
      rollbar:
        type: service
        id: Monolog\Handler\RollbarHandler
        level: warning
        channels: [ "!php" ]
```

Don't forget to add `ROLLBAR_ACCESS_TOKEN=` to your `.env` files.
