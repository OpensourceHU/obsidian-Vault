官方文档
[Try Docker Compose | Docker Documentation](https://docs.docker.com/compose/gettingstarted/)

## 杂项:
清理空间
`docker container rm -f $(docker container ls -aq)`

创建docker-compose.yml文件 并在此文件目录下执行:
`docker compose up`  会自动执行build操作
docker compose里面的服务会自动创建一个network


在使用前确保熟悉了
### [[json文件与yaml文件]]




