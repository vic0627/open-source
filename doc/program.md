# 程序運作與管理

本文件介紹了 Linux 中程序運作與管理的基本概念及操作指令，幫助無計算機基礎的學員理解相關知識並掌握基本操作技能。

---

## 認識程序

### 程式與程序的差別
- **程式**是儲存在硬碟上的執行檔，例如 `ls` 指令對應的程式文件。
- **程序**是指將程式載入記憶體並執行後的實例。
- Linux 系統透過程序來執行任務及管理記憶體。

### 程序的功能
- 控制硬體
- 提供服務
- 傳送封包
- 每個程序都有唯一的識別碼（PID，Process ID）。
- 系統啟動時，`init` 是第一個啟動的程序，其 PID 為 1。

---

## 父程序與子程序
Linux 中的程序可以分為父程序和子程序。

### 父子程序的生成方式
1. **`fork()` 系統調用**：創建子程序。
2. **系統啟動**：`init` 程序啟動後會生成其他系統程序。
3. **用戶交互**：通過 Shell 執行命令時，會創建子程序來執行命令。
4. **網絡服務**：處理客戶端請求時創建子程序。

---

## 程序管理基本命令

### 查詢程序
- `$ ps -e`：顯示當前用戶的所有程序。
- `$ ps -ef`：顯示所有程序的詳細信息。
- `$ ps -aux`：顯示更詳細的程序信息。
- 查詢特定程序：
  ```bash
  $ ps -aux | grep <process-name>
  ```
- 查詢特定程序的 PID：
  ```bash
  $ pidof <process-name>
  $ pgrep <process-name>
  ```

---

## 傳遞訊號
訊號是 Linux 中程序之間的通訊方式，用於通知程序執行特定操作。

### 常見訊號類型
- **終止程序**：
  - `SIGTERM (15)`：溫和地終止程序。
  - `SIGKILL (9)`：強制終止程序。
- **中斷操作**：`SIGINT (2)`，通常由 <kbd>Ctrl</kbd>+<kbd>C</kbd> 觸發。
- **暫停和恢復**：
  - `SIGSTOP (19)`：暫停程序。
  - `SIGCONT (18)`：恢復程序。

### 傳遞訊號的命令
- 使用 `kill`：
  ```bash
  $ kill -<訊號號碼或名稱> <PID>
  ```
- 使用 `killall`：
  ```bash
  $ killall -<訊號號碼或名稱> <程序名稱>
  ```
- 使用 `pkill`：
  ```bash
  $ pkill -<訊號號碼或名稱> <程序名稱>
  ```
- 在程式內部發送訊號：
  ```bash
  $ trap 'handle_signal SIGUSR1' SIGUSR1
  $ kill -SIGUSR1 $$
  ```

---

## 程序與工作

### 程序
- 程序是操作系統資源分配和任務管理的基本單位。
- 每個程序都有唯一的 PID。

### 工作（Job）
- 工作是一組相關任務或命令的集合。
- **前景與背景執行**：
  - **前景執行**：佔用終端。
  - **背景執行**：加上 `&` 可在背景執行。
  - 列出所有工作：
    ```bash
    $ jobs
    ```
  - 前景轉背景：
    ```bash
    $ bg %<工作編號>
    ```
  - 背景轉前景：
    ```bash
    $ fg %<工作編號>
    ```

---

## 實戰範例

### 實戰（ㄧ）：Apache 服務的操作
以 Google Colab Linux 環境為例：

1. 安裝 Apache：
   ```bash
   $ sudo apt install apache2
   ```
2. 啟用 Apache：
   ```bash
   $ sudo service apache2 start
   ```
3. 查詢 Apache 程序：
   ```bash
   $ ps -aux | grep apache
   ```
4. 擷取 Apache PID：
   ```bash
   $ pidof apache2
   ```
5. 強制終止 Apache：
   ```bash
   $ kill -SIGKILL $(pidof apache2)
   ```

### 實戰（二）：訊號傳遞
以 Google Colab Linux 環境為例，練習如何進行訊號的傳遞。

1. 執行 Apache 服務程序：
   ```bash
   $ sudo service apache2 start
   ```
2. 顯示 Apache 程序的 PID：
   ```bash
   $ pidof apache2
   ```
3. 強制終止 Apache 程序：
   ```bash
   $ kill -9 $(pidof apache2)
   ```
4. 顯示 Apache 程序的 PID：
   ```bash
   $ pidof apache2
   ```