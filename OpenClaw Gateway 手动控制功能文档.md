# OpenClaw Gateway 手动控制功能文档

## 1. 功能说明

OpenClaw Gateway 的手动控制功能允许用户通过命令行直接启动、停止或重启核心服务，无需通过图形界面操作。该功能适用于以下场景：
- 服务异常需要手动干预
- 系统维护时需要控制服务状态
- 自动化脚本中集成服务控制
- 故障排查时需要重启服务

## 2. 命令格式

| 命令 | 功能 |
|------|------|
| `openclaw gateway start` | 启动 OpenClaw Gateway 服务 |
| `openclaw gateway stop` | 停止 OpenClaw Gateway 服务 |
| `openclaw gateway restart` | 重启 OpenClaw Gateway 服务 |

## 3. 使用步骤

### 3.1 启动服务

**操作步骤：**
1. 打开命令行终端
2. 输入命令：`openclaw gateway start`
3. 按下回车键执行命令

**预期执行结果：**
- 终端显示启动进度信息
- 服务成功启动后显示 "Gateway service started successfully"
- 服务状态变为运行中

### 3.2 停止服务

**操作步骤：**
1. 打开命令行终端
2. 输入命令：`openclaw gateway stop`
3. 按下回车键执行命令

**预期执行结果：**
- 终端显示停止进度信息
- 服务成功停止后显示 "Gateway service stopped successfully"
- 服务状态变为已停止

### 3.3 重启服务

**操作步骤：**
1. 打开命令行终端
2. 输入命令：`openclaw gateway restart`
3. 按下回车键执行命令

**预期执行结果：**
- 终端显示停止和启动的进度信息
- 服务成功重启后显示 "Gateway service restarted successfully"
- 服务状态变为运行中

## 4. 注意事项

### 4.1 前提条件
- 已安装 OpenClaw 软件
- 网络连接正常
- 系统权限足够

### 4.2 权限要求
- Windows 系统：需要以管理员身份运行命令提示符
- Linux/macOS 系统：可能需要使用 sudo 权限

### 4.3 风险提示
- 停止服务会中断正在进行的任务，可能导致数据丢失
- 重启服务会暂时中断服务可用性
- 频繁重启可能影响服务稳定性

## 5. 常见问题

| 错误信息 | 可能原因 | 解决方法 |
|---------|---------|--------|
| "Permission denied" | 权限不足 | 使用管理员权限或 sudo 执行命令 |
| "Service not found" | 服务未安装或路径配置错误 | 重新安装 OpenClaw 或检查环境变量配置 |
| "Port already in use" | 端口被占用 | 检查是否有其他进程占用了相同端口，或修改配置文件中的端口设置 |
| "Service failed to start" | 配置错误或依赖服务未运行 | 检查配置文件和依赖服务状态 |

## 6. 示例说明

### 6.1 启动服务示例

**命令：**
```bash
openclaw gateway start
```

**输出结果：**
```
Starting OpenClaw Gateway service...
Initializing components...
Starting network services...
Gateway service started successfully
Status: Running
```

### 6.2 停止服务示例

**命令：**
```bash
openclaw gateway stop
```

**输出结果：**
```
Stopping OpenClaw Gateway service...
Gracefully shutting down components...
Gateway service stopped successfully
Status: Stopped
```

### 6.3 重启服务示例

**命令：**
```bash
openclaw gateway restart
```

**输出结果：**
```
Restarting OpenClaw Gateway service...
Stopping current service...
Starting new instance...
Gateway service restarted successfully
Status: Running
```

## 7. 相关命令

- `openclaw gateway status`：查询网关服务的当前运行状态
- `openclaw status`：查询 OpenClaw 主服务的整体运行状态
