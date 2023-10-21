# php_docker_dev
> 纯个人使用的PHP开发环境，可以自己根据个人需求再重新定制

```shell
docker compose -p dev up -d
```

> 如果使用 mongo nsql 或者 elastic 等等

```shell
docker compose --profile mongo --profile nsql --profile elastic -p dev up -d
```

> 配置文件是 `.env`

> 由于 `WORKDIR` 是 `/var/www/` 假如我们的开发目录是 `dev` 则工作目录对应的容器目录就是 `/var/www/dev`
> 所以我们使用 `composer` / `php` / `protoc` 等命令时可以指定工作空间如下

```shell
docker exec -it -w /var/www/dev php82 composer install # 来执行 composer install
```


