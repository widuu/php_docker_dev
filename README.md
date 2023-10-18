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
