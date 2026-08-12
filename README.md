# FPGA Logic Analyzer

用 Digilent Basys 3（`XC7A35T-1CPG236C`）搭建的逻辑分析仪，目标是抓取并解码
STM32 开发板上的 SPI 通信。

**当前状态：规划阶段，尚未编写任何 RTL。**
架构、约束条件和分阶段计划全部记录在 [`PROJECT_CONTEXT.md`](PROJECT_CONTEXT.md)。

## 目标规格

| 项目 | 规格 |
| --- | --- |
| 通道数 | 8（SPI 用 4：`SCK` / `MOSI` / `MISO` / `CS`） |
| 采样率 | 100 MSa/s（板载晶振直接采样） |
| 采样深度 | 约 200K 样本 ≈ 2 ms（受 BRAM 限制，板上无外部 RAM） |
| 探针接口 | Pmod `JA` / `JB` / `JC`，3.3V LVCMOS |
| 上位机链路 | 板载 USB-UART，目标波特率 921600 以上 |
| 上位机软件 | 初期 Python 导出 VCD；后期实现 SUMP 协议接入 PulseView |

推荐的 SCK 上限是 **10 MHz**（10 倍过采样）。杜邦线本身的边沿速率上限也在
10–30 MHz 区间，两者正好撞在一起。

## 目录结构

```text
src/           VHDL 源码
sim/           testbench
constraints/   Basys 3 XDC（Pmod 引脚分配）
host/          Python 上位机
scripts/       create_project.tcl / verify_project.tcl
```

Vivado 生成的工程和构建产物不纳入版本管理，一律由 `scripts/` 下的 Tcl 重建。

## 接线注意

- Basys 3 的 Pmod 是 **3.3V，不耐 5V**。STM32 同为 3.3V，可以直连。
- **地线要接多根。** 杜邦线没有每信号的地回流，Pmod 每口的 2 个 GND 都接上，
  能显著压低串扰和振铃。
- 只需要一根 micro-USB 线。板载 FT2232HQ 同时提供 JTAG 和 USB-UART 两个通道。

## 相关项目

前身是 [FPGA-RS232](https://github.com/hongxuanD/FPGA-RS232)：2025 年 LCSE
课程设计的 PIC 风格 8 位计算机，2026 年从 Nexys 4 DDR 迁移到 Basys 3 并完成实机验证。
本项目会借鉴其 UART 收发器结构和 Tcl 工程流程，细节见 `PROJECT_CONTEXT.md`。
