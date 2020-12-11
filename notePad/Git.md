### 1、git pull 报错 fatal: Authentication failed for 'xxx'
1. `git config --global --replace-all user.name "用户名"`
2. `git pull`
3. 这个时候会出现让输入用户，输入后，会又有输入密码，输入后，就正常了。

### 2、git push 时 error: RPC failed; HTTP 400 curl 55 Send failure: Connection was reset 
原因：http缓存不够
解决：
```bash
// 加大缓存
git config --global http.postBuffer 524288000
```
