cricket 蟋蟀、蛐蛐

# 公钥、私钥的生成
1. 生成1024位的公钥 `openssl genrsa -out private.pem 1024`
2. 根据公钥生成对应的私钥 `openssl rsa -in private.pem -pubout -out public.pem`
> 1024不够安全，可以用2048

# govendor 依赖管理
1. 初始化 `govendor init`
2. 获取特定版本的依赖 
	`govendor fetch github.com/dgrijalva/jwt-go@v3.2.0`
	`govendor fetch github.com/astaxie/beego/orm@v1.10.1`
	`govendor fetch github.com/lib/pq@v1.0.0`
	`govendor fetch github.com/go-redis/redis@v6.14.2`
	`govendor fetch github.com/gin-gonic/gin@v1.3.0`

# 运行
1. 最low的运行方式： `go run main.go models.go`
2. 运行package(相对于$GOPATH或$GOROOT): `go run github.com\kotocean\cricket\auth`
3. 测试：`cd auth/db && go test -v`

# 启动postgresql
`docker run --name postgresql -p 5432:5432 -e POSTGRESQL_USERNAME=my_user -e POSTGRESQL_PASSWORD=password123 -e POSTGRESQL_DATABASE=cricket_db bitnami/postgresql:10`
1. 登录 `psql cricket_db my_user`
2. 创建person表
```sql
create table person (
id serial,
name varchar(20),
password varchar(20)
);
```
3. 创建record表
```sql
create table record (
id serial,
client_mark varchar(320),
token varchar(320),
expires_at bigint,
valid boolean
);
```
4. 创建client表
```sql
create table client (
id serial,
client_id varchar(20),
secret varchar(128)
);
```

# 启动redis
`docker run --name redis -d -p 6379:6379 redis:alpine3.8`
默认没有密码，localhost登录

