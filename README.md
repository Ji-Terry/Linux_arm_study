# FreeRTOS + RISC-V / ARM 多架構 RTOS 實驗與開發平台

## 1. 專案簡介

本專案為針對嵌入式系統開發所建立的整合式作業平台，支援多種架構與開源作業系統模擬，並透過 Docker 統一開發環境與工具鏈管理。

### 支援平台架構

- QEMU_virt 虛擬平台
  - ARM + Linux Kernel(Done)
  - ARM + FreeRTOS (no)
  - RISC-V + Linux Kernel (no)
  - RISC-V + FreeRTOS（Done）
  
### 已完成功能

1. Docker-based 多平台控管與工具鏈整合
2. DTS / OpenSBI 整合與硬體啟動流程驗證
3. FreeRTOS RTOS 核心功能實作
   - OpenSBI 整合與硬體啟動流程驗證
   - 任務建立與優先權排程
   - 任務同步與中斷整合（Queue / Semaphore / Task Notification）
   - 記憶體與堆疊使用狀態監控
   - 客製化Uart設定
   - RISC-V 架構下自定中斷處理（trap handler / mtvec / CLINT）(on-going)
4. Linux Kernel 架構實作與驗證（於 QEMU ARM 平台）
   - 串接 HAL（Device Tree）、Kernel 驅動模組與 User-space 工具，建構完整 Linux 系統運作架構 
   - 開發自訂 Kernel Module，並支援與使用者工具互動
  
  
  


## 2. 開發環境與開源資源

### 開發平台
- 主機系統：macOS (M-series) + Docker 統一建構開發環境
- 模擬器：QEMU (支援 riscv32 / riscv64 / arm)

### Toolchain 支援
- [riscv64-unknown-elf-gcc](https://github.com/riscv-collab/riscv-gnu-toolchain)（for RISC-V 64-bit）
- [riscv32-unknown-elf-gcc](https://github.com/riscv-collab/riscv-gnu-toolchain)（for RISC-V 32-bit）
- riscv64-linux-gnu


### 作業系統與啟動元件
- FreeRTOS：RTOS 核心（[FreeRTOS GitHub](https://github.com/FreeRTOS/FreeRTOS)）
- Linux Kernel：預計支援（主線整合中）
- OpenSBI：支援 RISC-V 啟動流程（[OpenSBI GitHub](https://github.com/riscv-software-src/opensbi)）
- DTS / Linker script：用於設備樹配置與記憶體映射

### Dockerfile 預設工具（開發環境整合）
- Python / build-essential / git
- Toolchains（riscv / arm）
- QEMU / GDB / UART 模擬輸出工具


## 3. 開發環境與開建立

### <p>安裝</p>
<pre><code>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
</code></pre>
### <p>將Homebrew加入MacOS環境</p>
<pre><code>echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
</code></pre>
### <p>確認</p>
<pre><code>brew --version
</code></pre>
### <p>移除Homebrew</p>
<pre><code>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
</code></pre>


## 建立環境Linux kernel 編譯環境
### <p>安裝硬體模擬平台</p>
<pre><code>brew install git make wget ncurses qemu
</code></pre>

### <p>安裝Docker</p>
官方下載：https://www.docker.com/  
Docker相較於傳統VM省去OS，有減少容量和快速啟動等優勢很適合跨平台開發。
![alt text](image.png)
### <p>創立Docker的container</p>
<pre><code>  docker build -t linux-arm64-builder .
  docker run -it --rm -v $(pwd):/Project linux-arm64-builder /bin/bash
</code></pre>
