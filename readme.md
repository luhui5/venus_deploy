# Venus cluster deploy
先进行编译，自行查找官方文档

## 1. 运行venus-auth
```
nohup auth-server run > ~/auth.log 2>&1 &
```

## 2. 运行gateway
```
nohup venus-gateway --listen /ip4/0.0.0.0/tcp/45132 run \
--auth-url   http://127.0.0.1:8989 \
> venus-gateway.log 2>&1 &
```

## 3. 运行venus   daemon
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

## 4. 运行message
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

## 5. 启动wallet
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

## 6. gateway验证
![gatewayimage](gateway.png)


## 7. 初始化工作目录（--net来选择网络）
```
venus-sector-manager --net cali daemon init
```

### 更改 ~/.venus-sector-manager/sector-manager.cfg
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

## 8. 创建矿工号
```
venus-sector-manager --net cali util miner create --from=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --owner=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --worker=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --sector-size=32GiB
```

### 创建成功的信息
miner actor: f039791  这个就是矿工号
```
roots@worker101:~$ venus-sector-manager --net cali util miner create --from=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --owner=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --worker=t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq --sector-size=32GiB
2022-09-08T19:44:08.992+0800    DEBUG   policy  policy/const.go:20      NETWORK SETUP   {"name": "cali"}
2022-09-08T19:44:09.006+0800    INFO    cmd     internal/util_miner.go:167      constructing message    {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742"}
2022-09-08T19:44:09.009+0800    INFO    cmd     internal/global.go:206  wait for message receipt        {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m"}
2022-09-08T19:44:39.012+0800    INFO    cmd     internal/global.go:224  msg state: UnFillMsg    {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m"}
2022-09-08T19:44:39.012+0800    INFO    cmd     internal/global.go:206  wait for message receipt        {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m"}
2022-09-08T19:45:09.015+0800    INFO    cmd     internal/global.go:224  msg state: FillMsg      {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m"}
2022-09-08T19:45:09.015+0800    INFO    cmd     internal/global.go:206  wait for message receipt        {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m"}
2022-09-08T19:45:39.018+0800    INFO    cmd     internal/global.go:230  message landed on chain {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "mid": "bafy2bzacebkr43abg73sfsxzmvpjcgu5rlk3mzseglig3g7cp73ygqvkdw65m", "smcid": "bafy2bzaceclt6zz5csdbcjmo5runjacaarj3f4tmlhgubt2jrhnccmyka2vrg", "height": 1285891}
2022-09-08T19:45:39.018+0800    INFO    cmd     internal/util_miner.go:247      miner actor: f039791 (f2nueojrnvsu3myxccwfeys5icjf6kbmwdupcq35i)        {"size": "32GiB", "from": "t3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "actor": "f039742", "owner": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq", "worker": "f3s4ew3yoqx4bd27dkj4i5sw77mad2jlycorewwu43z32ytwwbwbte7y3sl4l3363ktjpiubsk5vmzgtn6x2kq"}
```

## 9. 更新~/.venus-sector-manager/sector-manager.cfg
[sector-manager.cfg](sector-manager.cfg)

## 10. 启动 poster
```
nohup venus-sector-manager --net cali  daemon run --poster > ~/poster.log 2>&1 &
```

## 11. 在另一台服务器上启动winner
### 挂载数据盘
应该要的吧，以防万一

### 拷贝poster上的sector-manager.cfg到 ~/.venus-sector-manager/sector-manager.cfg
`如果使用Chain = "/ip4/127.0.0.1/tcp/3453"等127.0.0.1的ip，记得修改为内网IP`
```

```