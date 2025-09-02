# SEHUATANG 爬虫系统

一个专为影视爱好者设计的磁力链接爬取、管理和推送工具。支持可视化番号库管理，提供搜索筛选和批量磁力链接复制功能，支持推送磁力链接到qBittorrent等下载器，以及115网盘离线下载。

### 🌐 在线演示

**演示网站**: [http://178.239.124.194:8000/](http://178.239.124.194:8000/)

> 您可以直接访问演示网站体验系统功能，默认密码：`admin123`

## 快速开始

### 系统要求

- Docker 20.10+
- Docker Compose 2.0+
- 至少1GB内存
- 至少10GB可用磁盘空间

### 生产环境部署

1. **创建部署目录**
```bash
mkdir sehuatang-crawler
cd sehuatang-crawler
```

2. **创建docker-compose.yml文件**
```yaml
version: '3.8'

services:
  sehuatang-crawler:
    image: wyh3210277395/sehuatang-crawler:latest
    container_name: sehuatang-crawler
    ports:
      - "8000:8000"
    environment:
      # 数据库配置
      - DATABASE_HOST=postgres
      - DATABASE_PORT=5432
      - DATABASE_NAME=sehuatang_db
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=postgres123
      
      # 应用配置
      - PYTHONPATH=/app/backend
      - ENVIRONMENT=production
      - ADMIN_PASSWORD=admin123
      
      # 代理配置（可选）
      - HTTP_PROXY=http://your-proxy:port
      - HTTPS_PROXY=http://your-proxy:port
      - NO_PROXY=localhost,127.0.0.1,192.168.0.0/16,10.0.0.0/8,172.16.0.0/12

    volumes:
      - sehuatang_data:/app/data
      - sehuatang_logs:/app/logs
    
    depends_on:
      - postgres
    
    restart: unless-stopped

  postgres:
    image: postgres:15-alpine
    container_name: sehuatang-postgres
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DB=sehuatang_db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres123
    
    volumes:
      - postgres_data:/var/lib/postgresql/data
    
    restart: unless-stopped

volumes:
  sehuatang_data:
  sehuatang_logs:
  postgres_data:

networks:
  default:
    name: sehuatang-network
```

3. **启动服务**
```bash
docker-compose up -d
```

4. **访问系统**
- 访问地址：http://localhost:8000
- 默认密码：admin123

### 配置说明

主要配置项（在docker-compose.yml中修改）：
- `ADMIN_PASSWORD` - 管理员密码（默认：admin123）
- `DATABASE_PASSWORD` - 数据库密码（默认：postgres123）
- `HTTP_PROXY` - HTTP代理（可选，如需要代理访问外网）
- `NO_PROXY` - 不走代理的地址（内网地址）

#### 下载器配置

在系统设置页面配置下载器连接信息：
- **qBittorrent**: 支持Web UI连接
- **Transmission**: 支持RPC连接
- **Aria2**: 支持JSON-RPC连接
- **115网盘**: 支持离线下载

## 管理命令

```bash
# 启动服务
docker-compose up -d

# 停止服务
docker-compose down

# 查看日志
docker-compose logs -f

# 重启服务
docker-compose restart
```

- 💬 **交流群**: [Telegram群组](https://t.me/sehuangtangcrawler)
- 🐳 **Docker镜像**: [wyh3210277395/sehuatang-crawler](https://hub.docker.com/r/wyh3210277395/sehuatang-crawler)

## 许可证

MIT License

## 免责声明

本项目仅供学习和研究使用，请遵守相关法律法规，不得用于商业用途。使用者需自行承担使用风险。！

⭐ 如果这个项目对你有帮助，请给我们一个St ar！
