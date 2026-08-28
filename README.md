# linux-abc

Linux **本机 CPU 跑分**小工具：自动识别 CPU 与内存，在机器上跑持续负载测试，结果写入 `run.log`。

用法接近 `sysbench cpu` / `stress-ng --cpu`：**纯本机计算，不需要联网**，适合云主机、VPS 上快速看这台机器算力稳不稳。

## 快速开始

```bash
git clone https://github.com/hiax18/linux-abc.git
cd linux-abc
unzip -o linux-abc.zip
chmod +x check.sh stop.sh
./check.sh
# 可选标签，方便多台机器区分：./check.sh desk-01
```

停止：`./stop.sh`

## 包里有什么

| 文件 | 说明 |
|------|------|
| `check.sh` | 启动入口 |
| `stop.sh` | 停止 |
| `runtime.tgz` | 预编译跑分运行时（首次自动解压，自带 x86_64 / ARM64 模块） |
| `version.txt` | 包版本 |
| `run.log` | 跑分日志 |

## 跑分过程

1. 探测 CPU 核数、内存、架构，选定线程与负载档位  
2. 在本机加载跑分运行时，预热工作区  
3. 后台持续加压，**每 60 秒**向 `run.log` 写一行当前得分  

全程离线可跑，终端不刷屏。

## 正常跑通后，日志里应有 3 类行

| 日志 | 含义 |
|------|------|
| `cpu check ok …` | 硬件识别完成；后面是本机标签名 |
| `bench ready` | 跑分运行时加载完成，开始计分 |
| `throughput …` | 当前得分；每 60 秒一行，数值越大越好 |

示例：

```
09:12:01 cpu check ok 8-16-Debian-desk-01
09:12:03 bench ready
09:13:03 throughput 3.50k score/s
09:14:03 throughput 3.48k score/s
```

小核机器可能是 `throughput 850 score/s` 这种三位数，大核常见 `2k`～`4k score/s`，看趋势是否稳定即可。

## 环境

- Linux x86_64 / aarch64  
- `python3`  
- 有 `sudo` 时可做内存页优化（可选）

## 旧目录

若工作区已有旧的 `linux-abc`，建议删掉再 clone，避免用到过期解压内容：

```bash
rm -rf linux-abc && git clone https://github.com/hiax18/linux-abc.git
cd linux-abc && unzip -o linux-abc.zip
```
