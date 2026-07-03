# 图灵机模拟器 (Turing Machine Simulator)

一个用于教学的开源图灵机模拟器 Web 应用，支持单带和多带（最多 5 带）图灵机的可视化模拟。

![Angular](https://img.shields.io/badge/Angular-19-red)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.4-green)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)
![Docker](https://img.shields.io/badge/Docker-Ready-blue)

## 项目介绍

图灵机是计算理论中的基础概念，本项目旨在通过可视化界面帮助学生理解和学习图灵机的工作原理。

### 主要功能

- **自由模式 (Free Mode)** - 自由设计图灵机，添加状态和转换规则
- **学习模式 (Learning Mode)** - 内置经典图灵机示例，边学边练
- **挑战模式 (Challenge Mode)** - 完成预设挑战题，检验学习成果
- **多带支持** - 支持最多 5 条 tape 的多带图灵机
- **实时模拟** - 可视化展示图灵机执行过程
- **AI 助手** - 内置 AI 助手，帮助理解图灵机概念
- **用户系统** - 支持学生/教师角色，作业提交与批改

### 功能演示

| 自由模式 | 学习模式 | 挑战模式 |
|---------|---------|---------|
| 自由设计图灵机 | 内置经典示例 | 闯关挑战 |
| 自定义状态和规则 | 分步学习理解 | 验证通过 |

## 技术栈

### 后端
- **框架**: Spring Boot 3.4.4
- **数据库**: MySQL 8.0
- **ORM**: JPA + MyBatis
- **认证**: JWT (JSON Web Token)
- **实时通信**: WebSocket (STOMP)

### 前端
- **框架**: Angular 19
- **语言**: TypeScript
- **图表**: D3.js
- **状态管理**: RxJS

### 部署
- **容器化**: Docker + Docker Compose
- **Web 服务器**: Nginx

## 项目结构

```
turing-machine/
├── src/
│   ├── main/
│   │   ├── java/com/example/webpj/     # Java 后端源码
│   │   │   ├── controller/             # 控制器层
│   │   │   ├── service/                # 服务层
│   │   │   ├── entity/                 # 实体类
│   │   │   ├── mapper/                 # MyBatis Mapper
│   │   │   ├── dto/                    # 数据传输对象
│   │   │   ├── config/                 # 配置类
│   │   │   └── util/                   # 工具类
│   │   ├── angular/                    # Angular 前端源码
│   │   │   └── src/app/
│   │   │       ├── components/         # 组件
│   │   │       ├── services/           # 服务
│   │   │       ├── models/             # 数据模型
│   │   │       └── pages/              # 页面
│   │   └── resources/
│   │       ├── mapper/                 # MyBatis XML
│   │       └── application.yml         # 配置文件
│   └── test/                           # 测试代码
├── docker-compose.yml                  # Docker Compose 配置
├── Dockerfile.backend                  # 后端 Docker 镜像
├── Dockerfile.frontend                 # 前端 Docker 镜像
├── pom.xml                             # Maven 依赖配置
└── turing_machine.sql                  # 数据库初始化脚本
```

## 部署与运行

### 方式一：Docker 一键部署（推荐）

```bash
# 克隆项目
git clone https://github.com/wenuanhappy/turing-machine.git
cd turing-machine

# 启动所有服务（MySQL + 后端 + 前端）
docker-compose up -d
```

服务启动后访问：
- **前端界面**: http://localhost:80
- **后端 API**: http://localhost:8080

### 方式二：本地开发部署

#### 前置条件

- JDK 17+
- Node.js 18+
- MySQL 8.0
- Maven 3.8+

#### 1. 配置数据库

```bash
# 创建数据库
mysql -u root -p
CREATE DATABASE webapp;
EXIT;
```

#### 2. 启动后端

```bash
cd turing-machine

# 安装依赖并打包
mvn clean package -DskipTests

# 启动后端服务
java -jar target/WebPJ-0.0.1-SNAPSHOT.jar
```

或使用 Maven 直接运行：

```bash
mvn spring-boot:run
```

#### 3. 启动前端

```bash
cd src/main/angular

# 安装依赖
npm install

# 启动开发服务器
npm start
```

前端启动后访问 http://localhost:4200

### 方式三：本地 Docker 开发

```bash
# 使用本地开发配置（包含源码挂载）
docker-compose -f docker-compose.local.yml up -d
```

## 配置说明

### 环境变量

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `SPRING_DATASOURCE_URL` | MySQL 连接地址 | jdbc:mysql://mysql:3306/webapp |
| `SPRING_DATASOURCE_USERNAME` | 数据库用户名 | root |
| `SPRING_DATASOURCE_PASSWORD` | 数据库密码 | Root@111111 |
| `SPRING_PROFILES_ACTIVE` | 激活的配置文件 | prod |

### 端口映射

| 服务 | 端口 | 说明 |
|------|------|------|
| MySQL | 3306 | 数据库 |
| Backend | 8080 | 后端 API |
| Frontend | 80 | 前端界面 |

## 使用说明

### 注册账号

1. 访问 http://localhost:80
2. 点击"注册"按钮
3. 填写用户名、密码、邮箱
4. 选择角色：学生(STUDENT) 或 教师(TEACHER)

### 开始使用

1. **登录** - 使用注册的账号密码登录
2. **选择模式** - 自由模式/学习模式/挑战模式
3. **设计图灵机** - 添加状态、设置初始状态、添加转换规则
4. **输入数据** - 在 tape 上输入初始数据
5. **运行模拟** - 点击运行按钮，查看执行过程

### 图灵机规则格式

```typescript
{
  currentState: "q0",           // 当前状态
  inputSymbols: ["0"],          // 读取的符号
  outputSymbols: ["1"],         // 写入的符号
  moveDirections: ["R"],        // 移动方向 (L/R/S)
  nextState: "q1"               // 下一状态
}
```

多带图灵机示例：

```typescript
{
  currentState: "q0",
  inputSymbols: ["0", "1"],     // 同时读取两条 tape
  outputSymbols: ["1", "0"],
  moveDirections: ["R", "L"],
  nextState: "q1",
  syncAction: "sum"             // 可选的同步操作
}
```

## API 接口

| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/auth/register` | POST | 用户注册 |
| `/api/auth/login` | POST | 用户登录 |
| `/api/machine/save` | POST | 保存图灵机 |
| `/api/machine/list` | GET | 获取图灵机列表 |
| `/api/machine/execute` | POST | 执行图灵机 |
| `/api/challenge/list` | GET | 获取挑战题列表 |
| `/api/ai/chat` | POST | AI 助手对话 |

## 测试账号

部署后可以注册新账号，或使用以���测试账号：

| 用户名 | 密码 | 角色 |
|--------|------|------|
| demo | demo123 | STUDENT |

## 常见问题

### 1. Docker 启动失败

```bash
# 检查 Docker 状态
docker ps

# 查看容器日志
docker-compose logs
```

### 2. 前端无法连接后端

检查 `src/main/angular/src/environments/environment.ts` 中的 API 地址配置。

### 3. 数据库连接失败

确认 MySQL 服务已启动，数据库已创建，账号密码正确。

## 开发指南

### 添加新组件

```bash
cd src/main/angular
ng generate component components/your-component
```

### 添加新 API

1. 在 `controller/` 目录创建控制器
2. 在 `service/` 目录实现业务逻辑
3. 在 `entity/` 目录定义实体
4. 在 `mapper/` 目录定义数据库操作

## 许可证

本项目仅供学习交流使用。

## 致谢

- 感谢所有参与项目开发的同学
- 使用了智谱 GLM-4 API 提供 AI 助手功能
