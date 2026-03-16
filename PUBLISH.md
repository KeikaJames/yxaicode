# @yxai/code NPM 发布指南

## 一、发布前准备

### 1. 注册 npm 账号
如果还没有 npm 账号，访问 https://www.npmjs.com/ 注册

### 2. 创建 npm 组织

由于包名使用了 scope（`@yxai/code`），需要先在 npm 上创建对应的组织：

1. 登录 https://www.npmjs.com/
2. 点击右上角头像 → Organizations
3. 点击 Create Organization
4. 选择 **Unlimited public packages**（免费）
5. 输入组织名称：`yxai`
6. 完成创建

> 如果不创建组织直接发布，会报错 `Scope not found`

### 3. 创建 Access Token

npm 要求使用双因素认证（2FA）或 Access Token 才能发布包：

1. 登录 https://www.npmjs.com/
2. 点击头像 → Access Tokens → Generate New Token
3. 选择 **Classic Token** → 类型选 **Automation**（自动绕过 2FA）
4. 复制生成的 token

```bash
npm config set //registry.npmjs.org/:_authToken=你的token
```

> 如果选了 Granular Access Token，需要勾选 "bypass 2fa" 权限，否则仍会报 403 错误

### 4. 登录 npm（可选，使用 Token 时可跳过）
```bash
npm login
```

### 5. 检查包名是否可用
```bash
npm view @yxai/code
```
如果显示 404，说明包名可用

## 二、本地测试

### 1. 安装依赖
```bash
npm install
```

### 2. 本地链接测试
```bash
# 链接到全局
npm link

# 测试命令
yxaiCode --help
yxaiCode --version
yxaiCode

# 测试完成后取消链接
npm unlink -g @yxai/code
```

### 3. 检查发布内容
```bash
# 预览将要发布的文件
npm pack --dry-run
```

确认只包含必要文件：
- `server.js` - 主程序
- `public/` - 前端静态文件
- `package.json`
- `README.md`

## 三、发布

### 1. 确保代码已提交
```bash
git add .
git commit -m "准备发布 v0.0.1"
```

### 2. 发布到 npm
```bash
# 首次发布（scope 包必须加 --access public）
npm publish --access public
```

看到 `+ @yxai/code@0.0.1` 表示发布成功

### 3. 验证发布
```bash
# 查看包信息
npm view @yxai/code

# 全局安装测试
npm install -g @yxai/code

# 运行
yxaiCode
```

## 四、版本更新

### 更新版本号
```bash
# 补丁版本 (0.0.1 -> 0.0.2)
npm version patch

# 小版本 (0.0.1 -> 0.1.0)
npm version minor

# 大版本 (0.0.1 -> 1.0.0)
npm version major
```

`npm version` 会自动修改 package.json 并创建 git tag

### 发布新版本
```bash
npm publish
```

## 五、常见问题

### 1. Scope not found（403）
原因：npm 上不存在 `@yxai` 这个组织

解决：登录 npm 网站 → Organizations → Create Organization → 创建 `yxai` 组织

### 2. Two-factor authentication required（403）
原因：npm 要求 2FA 或带 bypass 权限的 token

解决：创建 **Automation** 类型的 Classic Token（见上方步骤）

### 3. 端口占用（EADDRINUSE）
```bash
# Windows：查找并关闭占用进程
netstat -ano | findstr :3456
taskkill /F /PID <进程ID>

# Linux/Mac
lsof -i :3456
kill -9 <进程ID>

# 或使用其他端口
yxaiCode --port 8080
```

### 4. 包名冲突
如果 `@yxai/code` 已被占用，可以使用其他名称，如 `@yxai/code-ui`

### 5. 发布失败排查
```bash
# 确认登录状态
npm whoami

# 确认 token 配置
npm config get //registry.npmjs.org/:_authToken

# 确认 registry 地址（必须是官方源）
npm config get registry
# 应为 https://registry.npmjs.org/

# 如果使用了淘宝镜像，发布时需要指定官方源
npm publish --access public --registry https://registry.npmjs.org/
```

## 六、撤销发布

如果需要撤销发布（72 小时内）：
```bash
npm unpublish @yxai/code@0.0.1
```

> 注意：撤销后 24 小时内不能使用相同版本号重新发布

## 七、安全建议

1. 发布完成后，删除不再使用的 Access Token
2. 不要在代码或聊天中暴露 npm 密码和 token
3. 建议开启 npm 账号的 2FA 增强安全性
