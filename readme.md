# Venus cluster deploy
先进行编译，自行查找官方文档
## 1、运行venus-auth
`nohup auth-server run > ~/auth.log 2>&1 &`

## 2、运行gateway
```
nohup venus-gateway --listen /ip4/0.0.0.0/tcp/45132 run \
--auth-url   http://127.0.0.1:8989 \
> venus-gateway.log 2>&1 &
```

## 3、运行venus   daemon
```
nohup venus daemon --repodir=/worker-data/lotus --network=cali --auth-url http://127.0.0.1:8989 > venus.log 2>&1 &
```
### 设置密码
```
venus wallet set-password
```
### 生成钱包
```
venus wallet new
```
### 导出钱包
```
venus wallet export walletname
```
## 4、运行message
```
nohup venus-messager run \
--auth-url http://127.0.0.1:8989 \
--node-url /ip4/127.0.0.1/tcp/3453 \
--gateway-url /ip4/127.0.0.1/tcp/45132 \
--auth-token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY \
> msg.log 2>&1 &

nohup venus-messager run \
--auth-url http://127.0.0.1:8989 \
--node-url /ip4/127.0.0.1/tcp/3453 \
--gateway-url /ip4/127.0.0.1/tcp/45132 \
--node-token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY \
--gateway-token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY \
--auth-token  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY \
> ~/msg.log 2>&1 &
```

## 5、启动wallet
```
nohup venus-wallet run \
--gateway-api /ip4/127.0.0.1/tcp/45132 \
--gateway-token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY \
> wallet.log 2>&1 &
```


### 设置wallet密码
```
venus-wallet setpwd
```
### 导入密钥
```
venus-wallet import key
```
### 启用support wallet
```
venus-wallet support lovan(token name)
```

## 7、gateway验证
![gatewayimage](gateway.png)


## 8、初始化工作目录（--net来选择网络）
```
venus-sector-manager --net cali daemon init
```

## 更改 ~/.venus-sector-manager/sector-manager.cfg
更改内部url
```
[Common]
[Common.API]
Chain = "/ip4/127.0.0.1/tcp/3453"
Messager = "/ip4/0.0.0.0/tcp/39812"
#Market = "/ip4/{api_host}/tcp/{api_port}"
Gateway = ["/ip4/127.0.0.1/tcp/45132"]
Token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibG92YW4iLCJwZXJtIjoiYWRtaW4iLCJleHQiOiIifQ.Gu_AZ3S5mbmcYXB69-6SBJFaHY2SlOQ6BI0N0VhIapY"
```

## 9、创建矿工号
```
venus-sector-manager --net cali util miner create --from=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --owner=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --worker=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --sector-size=32GiB
```


