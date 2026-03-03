# 数据库配置指南

## 本地开发环境配置

### 1. 安装PostgreSQL

#### Windows
下载并安装 PostgreSQL: https://www.postgresql.org/download/windows/

#### macOS
```bash
brew install postgresql
brew services start postgresql
```

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

### 2. 创建数据库和用户

```sql
-- 连接到PostgreSQL
psql -U postgres

-- 创建数据库
CREATE DATABASE nope_game_db;

-- 创建用户（可选）
CREATE USER nope_user WITH PASSWORD 'your_password';

-- 授权
GRANT ALL PRIVILEGES ON DATABASE nope_game_db TO nope_user;

-- 退出
\q
```

### 3. 配置环境变量

复制 `.env.example` 为 `.env`：

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入你的数据库信息：

```env
# 数据库连接字符串
DATABASE_URL=postgresql://postgres:your_password@localhost:5432/nope_game_db

# JWT密钥（生产环境请使用强密钥）
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production

# 管理员密码
ADMIN_PASSWORD=nope2024

# 服务器端口
PORT=3000
```

### 4. 启动服务器

```bash
npm start
```

服务器会自动创建所需的数据库表。

---

## Railway部署配置

### 1. 连接PostgreSQL数据库

在Railway项目中：
1. 点击 "New" → "Database" → "PostgreSQL"
2. 等待数据库 provision 完成
3. Railway会自动设置 `DATABASE_URL` 环境变量

### 2. 配置其他环境变量

在Railway项目设置中添加：
- `JWT_SECRET`: 随机生成的强密钥
- `ADMIN_PASSWORD`: 管理员密码

### 3. 部署

推送代码到Railway，服务器会自动连接数据库并初始化表结构。

---

## 数据库表结构

应用会自动创建以下表：

- `users`: 用户账户
- `games`: 游戏列表
- `comments`: 游戏评论
- `follows`: 用户关注关系
- `messages`: 用户消息
- `activities`: 用户活动日志

---

## 故障排除

### 问题：连接被拒绝
- 确认PostgreSQL服务正在运行
- 检查端口5432是否被占用
- 验证用户名和密码

### 问题：权限不足
- 确保数据库用户有足够的权限
- 尝试使用postgres超级用户

### 问题：Railway部署失败
- 检查环境变量是否正确设置
- 查看Railway日志获取详细错误信息
