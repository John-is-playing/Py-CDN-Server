# 🚀 CDN管理系统使用说明书

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Python](https://img.shields.io/badge/python-3.7+-green.svg)
![License](https://img.shields.io/badge/license-MIT-orange.svg)
![Status](https://img.shields.io/badge/status-stable-brightgreen.svg)

> 高性能内容分发网络管理平台 - 基于Python Flask框架开发

---

## 📋 目录

- [系统概述](#-系统概述)
- [核心功能](#-核心功能)
- [安装部署](#-安装部署)
- [配置说明](#️-配置说明)
- [API接口](#-api接口)
- [使用示例](#-使用示例)
- [监控管理](#-监控管理)
- [故障排除](#-故障排除)
- [技术支持](#-技术支持)

---

## 📖 系统概述

CDN管理系统是一个基于Python Flask框架开发的高性能内容分发网络管理平台。该系统提供了完整的CDN节点管理、负载均衡、缓存控制和监控功能，适用于构建分布式内容分发网络。

> 💡 **设计理念**  
> 本系统采用微服务架构设计，支持水平扩展，具备高可用性和容错能力，能够有效提升内容分发效率和用户体验。

### 🎯 适用场景

- ✅ 大型网站内容分发加速
- ✅ 视频流媒体服务
- ✅ 静态资源缓存分发
- ✅ API接口负载均衡
- ✅ 文件下载加速服务

### 🏗️ 系统架构

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   用户请求      │───▶│  CDN管理系统    │───▶│   CDN节点群     │
│   Client        │    │  Load Balancer  │    │   Node Cluster  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                              │
                              ▼
                       ┌─────────────────┐
                       │   缓存系统      │
                       │   Cache System  │
                       └─────────────────┘
```

---

## ⭐ 核心功能

### 🌐 节点管理
- **节点注册**: 支持CDN节点的动态注册和注销
- **状态监控**: 实时监控节点健康状态和负载情况
- **故障转移**: 自动检测故障节点并进行流量切换

### ⚖️ 负载均衡
- **智能调度**: 根据节点负载和地理位置选择最优节点
- **权重分配**: 支持基于节点容量的权重负载均衡
- **地理就近**: 优先选择地理位置最近的节点

### 💾 缓存系统
- **多级缓存**: 支持内存缓存和分布式缓存
- **缓存策略**: 可配置的缓存过期时间和更新策略
- **缓存预热**: 支持内容预加载和缓存预热

### 🔄 请求转发
- **多协议支持**: 支持HTTP/HTTPS协议
- **多方法支持**: 支持GET、POST、PUT、DELETE等HTTP方法
- **流式传输**: 支持大文件和流媒体内容传输

### 💓 健康检查
- **心跳监控**: 定期检查节点心跳状态
- **自动恢复**: 自动检测和恢复故障节点
- **告警通知**: 节点异常时发送告警通知

### 📊 监控统计
- **实时监控**: 实时显示系统运行状态
- **性能统计**: 详细的性能指标和统计报表
- **日志记录**: 完整的操作日志和错误日志

---

## 🔧 安装部署

### 环境要求

| 项目 | 要求 | 推荐 |
|------|------|------|
| **Python版本** | 3.7+ | 3.9+ |
| **操作系统** | Linux/Windows/macOS | Ubuntu 20.04+ |
| **内存** | 512MB+ | 2GB+ |
| **磁盘空间** | 1GB+ | 5GB+ |
| **网络** | 稳定网络连接 | 千兆网络 |

### 快速安装

#### 步骤 1: 安装Python依赖

```bash
# 使用pip安装依赖包
pip install flask requests argparse

# 或使用requirements.txt（如果有的话）
pip install -r requirements.txt
```

#### 步骤 2: 下载系统文件

```bash
# 将main_server.py文件放置到目标目录
# 确保文件具有执行权限
chmod +x main_server.py
```

#### 步骤 3: 启动系统

```bash
# 使用默认配置启动
python main_server.py

# 或使用自定义配置启动
python main_server.py --host 0.0.0.0 --port 8080 --log-level DEBUG
```

### 🐳 Docker部署

```dockerfile
# Dockerfile示例
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY main_server.py .
EXPOSE 5000

CMD ["python", "main_server.py"]
```

```bash
# 构建和运行Docker容器
docker build -t cdn-system .
docker run -d -p 5000:5000 --name cdn-server cdn-system
```

---

## ⚙️ 配置说明

### 命令行参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `--host` | 字符串 | 0.0.0.0 | 服务监听主机地址 |
| `--port` | 整数 | 5000 | 服务监听端口 |
| `--node-timeout` | 整数 | 10 | 节点超时时间（秒） |
| `--health-interval` | 整数 | 5 | 健康检查间隔（秒） |
| `--cache-time` | 整数 | 30 | 默认缓存时间（秒） |
| `--disable-cache` | 布尔 | False | 禁用缓存功能 |
| `--log-level` | 字符串 | INFO | 日志级别 |
| `--log-file` | 字符串 | cdn_system.log | 日志文件名 |
| `--reset-config` | 布尔 | False | 重置配置为默认值 |

### 配置文件

系统会自动生成 `cdn_nodes.json` 配置文件来保存CDN节点信息：

```json
{
    "node1": {
        "url": "http://cdn1.example.com",
        "location": "Beijing",
        "capacity": 100,
        "heartbeat_interval": 10,
        "last_heartbeat": "2024-01-01T12:00:00",
        "is_active": true,
        "fail_count": 0,
        "current_load": 5
    },
    "node2": {
        "url": "http://cdn2.example.com",
        "location": "Shanghai",
        "capacity": 150,
        "heartbeat_interval": 10,
        "last_heartbeat": "2024-01-01T12:00:00",
        "is_active": true,
        "fail_count": 0,
        "current_load": 8
    }
}
```

### 环境变量配置

```bash
# 设置环境变量
export CDN_HOST=0.0.0.0
export CDN_PORT=5000
export CDN_LOG_LEVEL=INFO
export CDN_CACHE_ENABLED=true
```

---

## 🔌 API接口

### 节点管理接口

#### 注册CDN节点
- **接口**: `POST /register_node`
- **说明**: 注册或更新CDN节点
- **参数**:
  ```json
  {
    "id": "node1",
    "url": "http://cdn1.example.com",
    "location": "Beijing",
    "capacity": 100,
    "heartbeat_interval": 10
  }
  ```
- **响应**:
  ```json
  {
    "status": "Node registered successfully"
  }
  ```

#### 节点心跳
- **接口**: `POST /heartbeat/<node_id>`
- **说明**: 接收节点心跳信号
- **参数**: `node_id`（路径参数）
- **响应**:
  ```json
  {
    "status": "Heartbeat received"
  }
  ```

#### 查看节点状态
- **接口**: `GET /nodes`
- **说明**: 列出所有CDN节点状态
- **响应**:
  ```json
  {
    "nodes": [
      {
        "id": "node1",
        "url": "http://cdn1.example.com",
        "location": "Beijing",
        "capacity": 100,
        "current_load": 5,
        "status": "active",
        "last_heartbeat": "2024-01-01T12:00:00"
      }
    ]
  }
  ```

#### 缓存控制
- **接口**: `POST /toggle_cache`
- **说明**: 切换缓存功能状态
- **响应**:
  ```json
  {
    "status": "Cache is now enabled"
  }
  ```

### 内容分发接口

#### 获取内容
- **接口**: `GET /cdn/<path:file_path>`
- **说明**: 获取内容资源
- **特性**: 支持缓存、负载均衡、地理就近
- **请求头**:
  - `X-Client-Location`: 客户端位置信息（可选）

#### 上传内容
- **接口**: `POST /cdn/<path:file_path>`
- **说明**: 上传内容资源
- **支持**: 文件上传、JSON数据
- **Content-Type**: `application/json` 或 `multipart/form-data`

#### 更新内容
- **接口**: `PUT /cdn/<path:file_path>`
- **说明**: 更新内容资源
- **支持**: 文件更新、JSON数据

#### 删除内容
- **接口**: `DELETE /cdn/<path:file_path>`
- **说明**: 删除内容资源

---

## 💻 使用示例

### 1. 启动系统

```bash
# 基本启动
python main_server.py

# 自定义配置启动
python main_server.py \
  --host 0.0.0.0 \
  --port 8080 \
  --node-timeout 15 \
  --cache-time 60 \
  --log-level DEBUG
```

### 2. 注册CDN节点

```bash
curl -X POST http://localhost:5000/register_node \
  -H "Content-Type: application/json" \
  -d '{
    "id": "node1",
    "url": "http://cdn1.example.com",
    "location": "Beijing",
    "capacity": 100,
    "heartbeat_interval": 10
  }'
```

### 3. 发送心跳信号

```bash
# 节点定期发送心跳
curl -X POST http://localhost:5000/heartbeat/node1

# 使用脚本自动发送心跳
while true; do
  curl -X POST http://localhost:5000/heartbeat/node1
  sleep 5
done
```

### 4. 查看系统状态

```bash
# 查看所有节点状态
curl http://localhost:5000/nodes | jq

# 查看特定节点信息
curl http://localhost:5000/nodes | jq '.nodes[] | select(.id=="node1")'
```

### 5. 内容操作示例

```bash
# 获取文件
curl http://localhost:5000/cdn/images/logo.png -o logo.png

# 获取API数据
curl http://localhost:5000/cdn/api/users/123 \
  -H "X-Client-Location: Beijing"

# 上传JSON数据
curl -X POST http://localhost:5000/cdn/api/data \
  -H "Content-Type: application/json" \
  -d '{"key": "value", "timestamp": "2024-01-01T12:00:00"}'

# 上传文件
curl -X POST http://localhost:5000/cdn/upload/image.jpg \
  -F "file=@/path/to/image.jpg"

# 更新数据
curl -X PUT http://localhost:5000/cdn/api/config \
  -H "Content-Type: application/json" \
  -d '{"setting": "new_value"}'

# 删除资源
curl -X DELETE http://localhost:5000/cdn/temp/old_file.txt
```

### 6. 缓存管理

```bash
# 启用/禁用缓存
curl -X POST http://localhost:5000/toggle_cache

# 验证缓存效果（多次请求同一资源）
time curl http://localhost:5000/cdn/large_file.zip > /dev/null
time curl http://localhost:5000/cdn/large_file.zip > /dev/null
```

---

## 📊 监控管理

### 系统监控指标

| 指标类型 | 说明 | 监控方法 |
|----------|------|----------|
| **节点状态** | 活跃/非活跃节点数量统计 | `/nodes` 接口 |
| **负载情况** | 各节点当前负载和容量使用率 | 实时负载监控 |
| **响应时间** | 请求处理时间和转发延迟 | 日志分析 |
| **缓存命中率** | 缓存命中次数和命中率统计 | 缓存统计 |
| **错误统计** | 节点故障次数和错误类型 | 错误日志 |
| **流量统计** | 请求数量和数据传输量 | 流量监控 |

### 日志管理

系统提供详细的日志记录功能，日志文件默认保存为 `cdn_system.log`：

```bash
# 查看实时日志
tail -f cdn_system.log

# 查看错误日志
grep "ERROR" cdn_system.log

# 查看特定节点日志
grep "node1" cdn_system.log

# 查看请求统计
grep "Forwarding" cdn_system.log | wc -l

# 分析响应时间
grep "response_time" cdn_system.log | awk '{print $NF}' | sort -n
```

### 性能监控脚本

```bash
#!/bin/bash
# monitor.sh - 系统监控脚本

echo "=== CDN系统监控报告 ==="
echo "时间: $(date)"
echo

# 检查服务状态
if curl -s http://localhost:5000/nodes > /dev/null; then
    echo "✅ 服务状态: 正常"
else
    echo "❌ 服务状态: 异常"
fi

# 节点统计
NODES_INFO=$(curl -s http://localhost:5000/nodes)
ACTIVE_NODES=$(echo $NODES_INFO | jq '.nodes[] | select(.status=="active")' | jq -s length)
TOTAL_NODES=$(echo $NODES_INFO | jq '.nodes | length')

echo "📊 节点统计: $ACTIVE_NODES/$TOTAL_NODES 活跃"

# 系统资源
echo "💾 内存使用: $(free -h | grep Mem | awk '{print $3"/"$2}')"
echo "💿 磁盘使用: $(df -h / | tail -1 | awk '{print $3"/"$2" ("$5")"}')"

# 日志统计
ERROR_COUNT=$(grep "ERROR" cdn_system.log | wc -l)
echo "⚠️  错误数量: $ERROR_COUNT"

echo "========================"
```

### 告警配置

```bash
#!/bin/bash
# alert.sh - 告警脚本

# 检查节点健康状态
check_nodes() {
    INACTIVE_NODES=$(curl -s http://localhost:5000/nodes | jq '.nodes[] | select(.status=="inactive") | .id' | wc -l)
    if [ $INACTIVE_NODES -gt 0 ]; then
        echo "警告: 发现 $INACTIVE_NODES 个非活跃节点"
        # 发送邮件或短信告警
        # mail -s "CDN节点告警" admin@example.com < alert_message.txt
    fi
}

# 检查系统负载
check_load() {
    LOAD=$(uptime | awk -F'load average:' '{print $2}' | awk '{print $1}' | sed 's/,//')
    if (( $(echo "$LOAD > 5.0" | bc -l) )); then
        echo "警告: 系统负载过高 ($LOAD)"
    fi
}

check_nodes
check_load
```

---

## 🔧 故障排除

### 常见问题及解决方案

#### ❌ 问题 1: 节点无法注册

**症状**: 调用 `/register_node` 接口返回400错误

**可能原因**:
- 请求参数不完整或格式错误
- JSON格式不正确
- 缺少必需参数

**解决方案**:
```bash
# 检查JSON格式
echo '{
  "id": "node1",
  "url": "http://cdn1.example.com",
  "location": "Beijing",
  "capacity": 100
}' | jq .

# 确保包含所有必需参数
curl -X POST http://localhost:5000/register_node \
  -H "Content-Type: application/json" \
  -d '{
    "id": "node1",
    "url": "http://cdn1.example.com", 
    "location": "Beijing",
    "capacity": 100
  }'
```

#### ❌ 问题 2: 请求转发失败

**症状**: 返回502错误，日志显示"Failed to forward request"

**可能原因**:
- 目标节点不可达
- 网络连接超时
- 节点服务未启动

**解决方案**:
```bash
# 检查节点连通性
curl -I http://cdn1.example.com/health

# 检查节点状态
curl http://localhost:5000/nodes | jq '.nodes[] | select(.id=="node1")'

# 重启故障节点
ssh user@cdn1.example.com "sudo systemctl restart cdn-node"
```

#### ❌ 问题 3: 缓存不生效

**症状**: 相同请求每次都转发到后端节点

**可能原因**:
- 缓存功能被禁用
- 缓存时间设置过短
- 缓存键冲突

**解决方案**:
```bash
# 检查缓存状态
curl -X POST http://localhost:5000/toggle_cache

# 调整缓存时间
python main_server.py --cache-time 300

# 清理缓存（重启服务）
sudo systemctl restart cdn-system
```

#### ❌ 问题 4: 节点频繁离线

**症状**: 节点状态频繁在active和inactive之间切换

**可能原因**:
- 心跳超时时间设置过短
- 网络不稳定
- 节点负载过高

**解决方案**:
```bash
# 调整心跳超时时间
python main_server.py --node-timeout 30

# 检查网络延迟
ping -c 10 cdn1.example.com

# 检查节点负载
ssh user@cdn1.example.com "top -n 1"
```

### 性能优化建议

#### 🚀 缓存优化
```bash
# 根据内容类型设置不同缓存时间
# 静态资源: 长缓存时间
python main_server.py --cache-time 3600

# 动态内容: 短缓存时间  
python main_server.py --cache-time 60
```

#### 🌍 节点部署优化
- **地理分布**: 在不同地理位置部署CDN节点
- **容量规划**: 根据流量预估合理配置节点容量
- **网络优化**: 选择网络质量好的机房和线路

#### 📈 负载均衡优化
```python
# 自定义负载均衡策略
def find_best_node_custom(user_location=None):
    # 优先选择负载最低的节点
    # 其次考虑地理位置
    # 最后考虑节点容量
    pass
```

#### 🔍 监控优化
```bash
# 设置详细的监控指标
python main_server.py --log-level DEBUG

# 使用专业监控工具
# - Prometheus + Grafana
# - ELK Stack
# - Zabbix
```

### 系统维护

#### 定期维护任务

```bash
#!/bin/bash
# maintenance.sh - 系统维护脚本

echo "开始系统维护..."

# 1. 清理日志文件
find /var/log -name "*.log" -mtime +30 -delete
logrotate /etc/logrotate.conf

# 2. 检查磁盘空间
df -h | awk '$5 > 80 {print "警告: " $1 " 磁盘使用率过高 " $5}'

# 3. 更新节点配置
curl -s http://localhost:5000/nodes | jq . > nodes_backup.json

# 4. 检查系统资源
free -h
uptime

# 5. 重启服务（如需要）
# sudo systemctl restart cdn-system

echo "系统维护完成"
```

#### 备份恢复

```bash
# 备份配置文件
cp cdn_nodes.json cdn_nodes.json.backup.$(date +%Y%m%d)

# 备份日志文件
tar -czf logs_backup_$(date +%Y%m%d).tar.gz *.log

# 恢复配置
cp cdn_nodes.json.backup.20240101 cdn_nodes.json
sudo systemctl restart cdn-system
```

---

## 📞 技术支持

### 🆘 获取帮助

如果您在使用过程中遇到问题，可以通过以下方式获取帮助：

1. **查看日志**: 首先检查系统日志文件 `cdn_system.log`
2. **检查配置**: 确认配置文件 `cdn_nodes.json` 格式正确
3. **网络测试**: 验证网络连接和节点可达性
4. **版本检查**: 确认使用的是最新版本

### 📋 问题报告

报告问题时，请提供以下信息：

- **系统版本**: Python版本、操作系统版本
- **错误信息**: 完整的错误日志和堆栈跟踪
- **配置信息**: 启动参数和配置文件内容
- **重现步骤**: 详细的问题重现步骤
- **环境信息**: 网络环境、硬件配置等

### 📚 相关资源

- **官方文档**: [CDN系统文档](https://docs.example.com)
- **API参考**: [API接口文档](https://api.example.com)
- **最佳实践**: [部署指南](https://guide.example.com)
- **社区论坛**: [技术讨论区](https://forum.example.com)

---

## 📄 许可证

本项目采用 MIT 许可证，详情请参见 [LICENSE](LICENSE) 文件。

---

## 🤝 贡献

欢迎提交问题报告、功能请求和代码贡献！

### 贡献指南

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

---

## 📊 更新日志

### v1.0.0 (2024-01-01)
- ✨ 初始版本发布
- 🌐 支持CDN节点管理
- ⚖️ 实现负载均衡功能
- 💾 添加缓存系统
- 📊 集成监控功能

---

<div align="center">

**© 2024 CDN管理系统 | 版本 1.0.0**

*如需技术支持，请联系系统管理员*

[![GitHub](https://img.shields.io/badge/GitHub-项目地址-blue?logo=github)](https://github.com/John-is-playing/Py-CDN-Server/)
QQ: 105297842

</div>
