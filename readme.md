# Venus cluster deploy
先进行编译，自行查找官方文档

## 1. venus-auth
运行venus-auth  
默认目录 ~/.venus-auth  或者 ~/.auth_home  
默认监听端口8989
```
nohup venus-auth run > ~/auth.log 2>&1 &
```

生成token
```
venus-auth token gen --perm admin sometoken
```
查看所拥有的token
```
venus-auth token list
```
[auth_token](images/auth_token.jpg)
添加user
```
venus-auth user add someuser
```
list user
```
~$ venus-auth user list
number: 1
name: someuser
state: enabled
createTime: Sat, 10 Sep 2022 18:18:44 CST
updateTime: Sat, 10 Sep 2022 18:18:44 CST
```
设置 user 可用,否则在其他组件请求 user 列表时请求不到.
```
 venus-auth user update --name=someuser --state=1
```
## 2. gateway
默认端口 45132  
默认目录 ~/.venusgateway/  
```
nohup venus-gateway --listen /ip4/0.0.0.0/tcp/45132 run \
--auth-url   http://127.0.0.1:8989 \
> ~/venus-gateway.log 2>&1 &
```

## 3. 运行venus   daemon
```
nohup venus daemon \
--repodir=/worker-data/lotus \
--network=cali \
--auth-url http://127.0.0.1:8989 \
> ~/venus.log 2>&1 &
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
### 修改监听端口 ~/.venus/config.json，修改完成后重启venus
```
"apiAddress": "/ip4/0.0.0.0/tcp/3453",
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
### 修改监听ip ~/.venus_wallet/config.toml
```
ListenAddress = "/ip4/0.0.0.0/tcp/5678/http"
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

## 10. 启动 venus-sector-manager(分离可能有问题，暂时先这样)
详情阅读： [https://github.com/ipfs-force-community/venus-cluster/blob/main/docs/zh/09.%E7%8B%AC%E7%AB%8B%E8%BF%90%E8%A1%8C%E7%9A%84poster%E8%8A%82%E7%82%B9.md](https://github.com/ipfs-force-community/venus-cluster/blob/main/docs/zh/09.%E7%8B%AC%E7%AB%8B%E8%BF%90%E8%A1%8C%E7%9A%84poster%E8%8A%82%E7%82%B9.md)
配置并启动源节点
```
nohup venus-sector-manager --net cali daemon run --miner > ~/win.log 2>&1 &
```
现在配置另一台机器
```
# 拷贝源节点的 ~/.venus-sector-manager/sector-manager.cfg

# 修改Chain = "/ip4/127.0.0.1/tcp/3453"等为内网地址

# 先init出配置文件
venus-sector-manager --home=~/.venus-individual-poster daemon init

# 修改配置文件 ext-prover.cfg
[[WdPost]]
[WdPost.Envs]
CUDA_VISIBLE_DEVICES = "0"
TMPDIR = "/tmp/ext-prover0/"

# 启动prover
# venus-sector-manager --home=~/.venus-individual-poster daemon run --proxy="127.0.0.1:1789" --poster --listen=":2789" --conf-dir="~/.venus-sector-manager" --ext-prover
nohup venus-sector-manager --home ~/.venus-sector-manager2 daemon run --proxy="192.168.4.101:1789" --listen=":2789" --conf-dir="~/.venus-sector-manager" --poster > poster.log 2>&1 &
```

## 11. 安装必要工具
```
# 安装必要的工具. 
sudo apt install -y wget make gcc
# 下载 hwloc-2.7.1.tar.gz
wget https://download.open-mpi.org/release/hwloc/v2.7/hwloc-2.7.1.tar.gz

tar -zxpf hwloc-2.7.1.tar.gz
cd hwloc-2.7.1
./configure --prefix=/usr/local
make -j$(nproc)
sudo make install
ldconfig /usr/local/lib

sudo apt install ocl-icd-opencl-dev
```

## 11.venus-worker
### 计算最优解的官方文档
[https://github.com/ipfs-force-community/venus-cluster/blob/main/docs/zh/12.venus-worker-util.md](https://github.com/ipfs-force-community/venus-cluster/blob/main/docs/zh/12.venus-worker-util.md)
### 拷贝必要的包去worker
```
sudo scp username@ip:/home/roots/venus/venus-cluster/dist/bin/venus* /usr/local/bin/
```
### 配置 venus-worker.toml
[venus-worker.toml](venus-worker.toml)

### 12. 启动worker
配置sealing  dir
```
venus-worker store sealing-init -l <dir1> <dir2> <dir3> <...>
```

配置持久化数据目录
```
venus-worker store file-init -l <dir1>
```

配置tree-d
```
venus-worker generator tree-d -p /worker-data/32g -s 32GiB
```

启动worker
```
nohup venus-worker daemon -c venus-worker.toml > worker.log 2>&1 &
```