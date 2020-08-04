## 实战MySQL用户管理

1. MySQL用户名介绍
2. MySQL权限管理
3. 为什么我用root连接不上数据库，连接上提示权限不对
4. 利用精准匹配拒绝用户连接
5. 为什么我对用户的表授权后立马会生效，禁用super权限后却需要重新登陆
6. 不能登陆MYSQL的分析方法
7. 账号安全相关

### 1. MySQL用户名介绍

用户名@主机 + 密码

```sql
create user 用户名@主机 identified by '密码';

help create user;
```

用户名举例

ylh@localhost
ylh@10.10.120.%
ylh@%
ylh@120.79.225.34

show grants for 'root'@'192.168.0.2';
show grants for 'root'@'localhost';




