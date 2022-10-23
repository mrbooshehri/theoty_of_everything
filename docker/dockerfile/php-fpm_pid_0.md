# how to run php-fpm in the foreground so it could be PID 1 in a docker container

```bash
php-fpm -F -R
```

## Explanation
We can check the available options with php-fpm --help

```bash
-F, --nodaemonize
      force to stay in foreground, and ignore daemonize option from config file
```
If you are running php-fpm in a docker container, there is a good chance you are running the process as root. php-fpm won't start as root without an extra flag:

```bash
  -R, --allow-to-run-as-root
        Allow pool to run as root (disabled by default)
```

Tags:
```
#docker #dockerfile #php-fpm
```

Related:
```
* https://stackoverflow.com/questions/57702705/docker-php-fpm-container-exit-with-code-0-after-running-symfony-console-commands
* https://stackoverflow.com/questions/37313780/how-can-i-start-php-fpm-in-a-docker-container-by-default
* https://stackoverflow.com/questions/40047329/docker-php5-fpm-service-exited-code-0
```
