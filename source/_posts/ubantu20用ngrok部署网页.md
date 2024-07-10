---
title: ubantu20通过ngrok部署网页
categories:
  - web学习笔记
---


# 使用 ngrok 部署 Node.js 应用的完整流程

## 1. 环境准备

### 1.1 安装 Node.js 和 npm
确保你的系统上安装了 Node.js 和 npm。如果没有安装，可以通过以下命令安装：
```
sudo apt update
sudo apt install nodejs npm
```

### 1.2 安装 MySQL
确保安装了 MySQL 数据库服务器：
```
sudo apt update
sudo apt install mysql-server
```

## 2. 配置 MySQL

### 2.1 登录 MySQL
使用 root 用户登录 MySQL：
```
sudo mysql -u root
```

### 2.2 设置 root 用户密码和权限
在 MySQL 控制台中，设置 root 用户密码和权限：
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';
FLUSH PRIVILEGES;
```

### 2.3 创建数据库和表
在 MySQL 中创建你的数据库和表：
```
CREATE DATABASE batteries;
USE batteries;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
);

CREATE TABLE battery_data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    temperature VARCHAR(255) NOT NULL,
    voltage VARCHAR(255) NOT NULL,
    current VARCHAR(255) NOT NULL,
    soc VARCHAR(255) NOT NULL
);
```

## 3. 配置 Node.js 应用

### 3.1 项目目录结构
确保你的项目目录结构如下：
```
test/
|-- public/
|   |-- login.html
|   |-- index.html
|   |-- images/
|       |-- dc.png
|       |-- dc_yellow.png
|       |-- dc_kong.png
|-- server.js
```

### 3.2 安装项目依赖
在项目目录中创建 package.json 并安装必要的依赖：
```
cd ~/桌面/test
npm init -y
npm install express body-parser express-session sequelize mysql2
```

### 3.3 配置 server.js
创建并配置 server.js 文件，确保正确连接到 MySQL 数据库并设置路由。

### 3.4 运行 Node.js 应用
确保在项目目录中运行你的 Node.js 应用：
```
node server.js

你应该看到类似 Server is running at http://localhost:3000 的消息，表示服务器已经成功启动。
```

## 4. 使用 ngrok 暴露本地服务器

### 4.1 安装和配置 ngrok
如果还没有安装 ngrok，可以从 ngrok 官方网站 下载并安装。然后，通过以下命令配置你的 ngrok authtoken：
```
ngrok config add-authtoken <your-authtoken>
```

### 4.2 启动 ngrok
运行以下命令来启动 ngrok，并将本地服务器暴露到互联网：
```
ngrok http 3000

你将会看到一个公共 URL，例如 http://abcdef1234.ngrok.io，通过这个 URL 你可以从外网访问你的本地服务器。
```

## 5. 验证和测试

### 5.1 本地访问
打开浏览器，输入 http://localhost:3000，检查是否能够正常访问你的应用。

### 5.2 外网访问
使用 ngrok 提供的 URL，在浏览器中输入这个 URL，检查是否能够正常访问你的应用。

## 6. 遇到的问题及解决方案

### 问题 1: MySQL 密码不符合策略要求
现象: 设置 root 用户密码时出现错误 ERROR 1819 (HY000): Your password does not satisfy the current policy requirements。

解决方案:

使用复杂密码: 尝试使用包含大写字母、小写字母、数字和特殊字符的密码。
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NewP@ssw0rd!';
FLUSH PRIVILEGES;
```

降低密码策略要求:
```
SET GLOBAL validate_password.policy = LOW;
SET GLOBAL validate_password.length = 4;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;
```

### 问题 2: Node.js 无法找到 server.js
现象: 运行 node server.js 时出现错误 Cannot find module '/home/username/桌面/server.js'。

解决方案:

确认文件路径正确。
确保在项目目录中运行命令。
```
cd ~/桌面/test
node server.js
```

### 问题 3: Sequelize 连接 MySQL 失败
现象: Sequelize 无法连接到 MySQL，出现错误 Access denied for user 'root'@'localhost'。

解决方案:

确认 MySQL 用户名和密码正确，并且有足够权限。
修改 server.js 中的 Sequelize 配置，确保数据库名、用户名、密码和主机名正确。
```
const sequelize = new Sequelize('batteries', 'root', '1234', {
    host: 'localhost',
    dialect: 'mysql'
});
```

### 问题 4: MySQL 连接被拒绝
现象: 使用 mysql -u root -p 登录时，出现错误 Access denied for user 'root'@'localhost'。

解决方案:

确认 MySQL 服务正在运行:
```
sudo systemctl status mysql
sudo systemctl start mysql
```

使用 sudo 登录 MySQL:
```
sudo mysql -u root
```

重新设置 root 用户密码:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;
```

### 问题 5: 部署 ngrok 后访问失败
现象: 使用 ngrok 提供的 URL 无法访问应用。

解决方案:

确保本地服务器正在运行。
```
node server.js
```

启动 ngrok 并确认 URL。
```
ngrok http 3000
```

通过这些步骤，你应该能够成功地在本地部署一个 Node.js 和 MySQL 应用，并通过 ngrok 将其暴露到互联网上。

# 步骤 1: 创建数据库和表

## 登录MySQL：
打开终端并登录MySQL：
```
sudo mysql -u root -p
```

## 创建数据库：
如果你还没有创建数据库 batteries，可以使用以下命令：
```
CREATE DATABASE batteries;
USE batteries;
```

## 创建表：
如果你还没有创建用户表 users 和电池数据表 battery_data，可以使用以下命令：
```
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

CREATE TABLE battery_data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    temperature VARCHAR(255) NOT NULL,
    voltage VARCHAR(255) NOT NULL,
    current VARCHAR(255) NOT NULL,
    soc VARCHAR(255) NOT NULL
);

# 步骤 2: 插入示例数据

## 插入用户数据：
INSERT INTO users (username, password) VALUES ('admin', 'adminpassword');
INSERT INTO users (username, password) VALUES ('user1', 'user1password');

## 插入电池数据：
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery1', '25', '3.7', '1.5', '80');
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery2', '30', '3.8', '1.6', '60');
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery3', '27', '3.6', '1.4', '90');

# 完整的SQL示例
-- 登录MySQL并选择数据库
USE batteries;

-- 创建表
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

CREATE TABLE battery_data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    temperature VARCHAR(255) NOT NULL,
    voltage VARCHAR(255) NOT NULL,
    current VARCHAR(255) NOT NULL,
    soc VARCHAR(255) NOT NULL
);

-- 插入用户数据
INSERT INTO users (username, password) VALUES ('admin', 'adminpassword');
INSERT INTO users (username, password) VALUES ('user1', 'user1password');

-- 插入电池数据
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery1', '25', '3.7', '1.5', '80');
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery2', '30', '3.8', '1.6', '60');
INSERT INTO battery_data (name, temperature, voltage, current, soc) VALUES ('Battery3', '27', '3.6', '1.4', '90');

# 验证数据
在插入数据后，可以使用以下命令来验证数据是否正确插入：

SELECT * FROM users;
SELECT * FROM battery_data;
```
