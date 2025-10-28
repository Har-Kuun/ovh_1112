# 🪟 Windows 运行 Docker 指南

## 📋 前置要求

### 1. 安装 Docker Desktop

**下载地址：** https://www.docker.com/products/docker-desktop/

**安装步骤：**
1. 下载 Docker Desktop for Windows
2. 运行安装程序
3. 重启电脑
4. 启动 Docker Desktop

**验证安装：**
```powershell
docker --version
docker-compose --version
```

应显示版本号，如：
```
Docker version 24.0.x
Docker Compose version v2.x.x
```

---

## 🚀 运行项目

### 方式一：使用 docker-compose（推荐）

#### 1. 打开 PowerShell

在项目目录右键 → "在终端中打开"

#### 2. 启动

```powershell
docker-compose up -d --build
```

#### 3. 访问

http://localhost:8080

---

### 方式二：纯 Docker 命令

#### 1. 构建镜像

```powershell
docker build -t ovh-phantom-sniper .
```

#### 2. 启动容器

```powershell
docker run -d `
  --name ovh-sniper `
  -p 5000:5000 `
  -p 8080:8080 `
  -v ${PWD}\backend\.env:/app/backend/.env `
  -v ${PWD}\backend\data:/app/backend/data `
  -v ${PWD}\backend\logs:/app/backend/logs `
  -v ${PWD}\backend\cache:/app/backend/cache `
  -e API_SECRET_KEY=123456 `
  ovh-phantom-sniper
```

#### 3. 访问

http://localhost:8080

---

### 方式对比

| 方式 | 命令复杂度 | 推荐 |
|------|-----------|------|
| docker-compose | ✅ 简单 | ⭐⭐⭐⭐⭐ |
| 纯 docker | ❌ 复杂 | ⭐⭐⭐ |

**推荐使用 docker-compose！** ✨

---

## ⚙️ 配置

### 修改配置文件

用记事本打开：
```
C:\Users\video\Desktop\OVH\backend\.env
```

修改：
```env
API_SECRET_KEY=your-key-here
PORT=5000
DEBUG=false
```

### 重启容器

```powershell
docker-compose restart
```

---

## 📋 常用命令

### docker-compose 命令（推荐）

```powershell
# 启动
docker-compose up -d

# 停止
docker-compose stop

# 重启
docker-compose restart

# 查看日志
docker-compose logs -f

# 查看状态
docker-compose ps

# 删除容器
docker-compose down

# 重新构建
docker-compose up -d --build
```

### 纯 docker 命令

```powershell
# 启动容器
docker start ovh-sniper

# 停止容器
docker stop ovh-sniper

# 重启容器
docker restart ovh-sniper

# 查看日志
docker logs -f ovh-sniper

# 进入容器
docker exec -it ovh-sniper /bin/sh

# 删除容器
docker rm -f ovh-sniper

# 删除镜像
docker rmi ovh-phantom-sniper
```

---

## 🔧 修改前端端口

**文件：** `vite.config.ts` 第 10 行

```typescript
server: {
  port: 8080,  // 改成其他端口
}
```

**重新构建：**
```powershell
docker-compose down
docker-compose up -d --build
```

---

## 🐛 常见问题

### Q: Docker Desktop 启动失败？

**A:** 
1. 确保启用了 WSL 2
2. 在 BIOS 中启用虚拟化
3. 以管理员权限运行 Docker Desktop

### Q: 端口被占用？

**A:** 
```powershell
# 查看端口占用
netstat -ano | findstr :8080
netstat -ano | findstr :5000

# 修改 docker-compose.yml
ports:
  - "8081:8080"  # 改成其他端口
```

### Q: 构建很慢？

**A:** 
- 首次构建需要下载镜像（5-10分钟）
- 后续构建会快很多（使用缓存）
- 可以配置 Docker 镜像加速器

### Q: 修改代码后如何更新？

**A:** 
```powershell
# 停止容器
docker-compose down

# 重新构建（会包含新代码）
docker-compose up -d --build
```

### Q: 如何查看容器内的文件？

**A:** 
```powershell
# 进入容器
docker exec -it ovh-phantom-sniper /bin/sh

# 查看文件
ls -la backend/
cat backend/.env
exit
```

---

## 📊 Docker Desktop 管理

### 图形界面操作

1. 打开 Docker Desktop
2. 点击 "Containers" 标签
3. 可以看到 `ovh-phantom-sniper` 容器
4. 点击可以：
   - 查看日志
   - 停止/启动
   - 删除容器

---

## ✅ 快速命令总结

```powershell
# 一键启动
docker-compose up -d --build

# 访问
http://localhost:8080

# 修改配置
# 1. 编辑 backend\.env
# 2. docker-compose restart

# 查看日志
docker-compose logs -f

# 停止
docker-compose down
```

---

**Windows 下 Docker 就这么用！** 🪟🐳

