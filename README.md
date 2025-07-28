# 🚀 Tusky Bot 
## 📋 概述

Tusky Bot 特性：
- **🎯 每日随机调度** - 模拟真人行为，避免检测
- **🔒 一对一代理配置** - 每个账户使用独立代理
- **⚙️ JSON 配置管理** - 简单直观的配置方式
- **🔄 持续每日运行** - 每天自动重新调度

## 📁 配置文件格式

程序使用 `config.json` 文件进行配置

### 🏗️ 配置文件结构

```json
{
  "accounts": [
    {
      "privateKey": "你的私钥",
      "mnemonic": "你的助记词",
      "name": "账户名称",
      "proxyUrl": "http://127.0.0.1:7890",
      "enabled": true
    }
  ],
  "settings": {
    "uploadsPerDay": 1,
    "dailyCycles": 10,
    "randomizeSchedule": true,
    "waitBetweenUploads": 2000,
    "waitBetweenAccounts": 5000,
    "defaultProxyUrl": "http://127.0.0.1:7890"
  }
}
```

## 👤 账户配置 (accounts)

每个账户对象包含以下字段：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| **privateKey** | string | 二选一 | Sui 私钥（与 mnemonic 二选一） |
| **mnemonic** | string | 二选一 | 助记词（与 privateKey 二选一） |
| **name** | string | 可选 | 账户名称，用于日志显示 |
| **proxyUrl** | string | 可选 | 该账户专属代理地址 |
| **enabled** | boolean | 可选 | 是否启用该账户（默认：true） |

### 💡 配置示例

**使用私钥的账户：**
```json
{
  "privateKey": "suiprivkey1qxxx...",
  "mnemonic": null,
  "name": "主账户",
  "proxyUrl": "http://127.0.0.1:7890",
  "enabled": true
}
```

**使用助记词的账户：**
```json
{
  "privateKey": null,
  "mnemonic": "word1 word2 word3 word4 word5 word6 word7 word8 word9 word10 word11 word12",
  "name": "备用账户",
  "proxyUrl": "http://192.168.1.100:7891",
  "enabled": true
}
```

**多账户配置示例：**
```json
{
  "accounts": [
    {
      "privateKey": "suiprivkey1qxxx...",
      "name": "账户1",
      "proxyUrl": "http://127.0.0.1:7890",
      "enabled": true
    },
    {
      "privateKey": "suiprivkey1qyyy...",
      "name": "账户2",
      "proxyUrl": "http://192.168.1.100:7020",
      "enabled": true
    },
    {
      "mnemonic": "word1 word2 word3...",
      "name": "账户3",
      "proxyUrl": "http://192.168.1.101:8080",
      "enabled": false
    }
  ]
}
```

## ⚙️ 设置配置 (settings)

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| **uploadsPerDay** | number | 1 | 每次运行时每个账户的上传次数 |
| **dailyCycles** | number | 1 | 每天运行次数 |
| **randomizeSchedule** | boolean | false | 是否随机化运行时间 |
| **waitBetweenUploads** | number | 2000 | 同一账户上传间隔时间（毫秒） |
| **waitBetweenAccounts** | number | 5000 | 不同账户间处理间隔时间（毫秒） |
| **defaultProxyUrl** | string | http://127.0.0.1:7890 | 默认代理服务器地址 |

## 🎯 每日随机调度功能

这是 Tusky Bot 的核心创新功能，让程序行为完全模拟真人：

### ✨ 核心特性

| 特性 | 说明 | 优势 |
|------|------|------|
| **智能首次运行** | 程序启动后立即执行第一次任务 | 无需等待，立即开始工作 |
| **随机时间分布** | 后续任务在剩余时间内随机分布 | 避免固定模式，降低检测风险 |
| **每日重新调度** | 每天生成全新的随机时间表 | 每天都有不同的行为模式 |
| **精确时间控制** | 精确等待到每个目标时间 | 确保按计划执行 |

### 🎲 调度逻辑示例

假设当前时间是 **22:20**，设置 `dailyCycles: 10`：

```
=== Daily Schedule for Today ===
Run 1: NOW (immediate)     ← 立即执行
Run 2: 22:29:51           ← 9分钟后
Run 3: 23:17:34           ← 57分钟后
Run 4: 23:31:59           ← 1小时11分钟后
Run 5: 23:35:04           ← 1小时15分钟后
...
Run 10: 23:48:00          ← 1小时28分钟后
```
