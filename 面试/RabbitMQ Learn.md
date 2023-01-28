docker Compose File:
```yaml
version: "3.2"
services:
  rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: 'rabbitmq'
    ports:
        - 5672:5672
        - 15672:15672
    volumes:
        - ~/.docker-conf/rabbitmq/data/:/var/lib/rabbitmq
        - ~/.docker-conf/rabbitmq/log/:/var/log/rabbitmq
    networks:
        - rabbitmq_go_net

networks:
  rabbitmq_go_net:
    driver: bridge
```



使用方法  docker compose up

可能遇到的问题: 无法创建日志文件夹导致启动失败 参考: [rabbitmq-server fails to start when log directory does not exist · Issue #1151 · rabbitmq/rabbitmq-server (github.com)](https://github.com/rabbitmq/rabbitmq-server/issues/1151)

rabbitMQ的日志创建是起一个临时用户来创建文件夹, 可能会遇到权限问题   所以不能创建在需要root权限的路径下

启动后, 访问localhost的15672端口   用户名密码均为:guest   就进入了rabbitMQ的后台系统

