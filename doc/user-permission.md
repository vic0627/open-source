# 使用者權限管理

## 使用者分類
Linux 是**多人系統**，意旨多位使用者共用一台電腦開發為前提，角色一般分成：

### 系統管理員
- 名稱為 root，個人目錄為 `/root`，有最高許可權，可執行任何操作。
- `sudo` 可讓一般使用者臨時取得 root 權限，但需在 `/etc/sudoers` 中設定。

#### 常用指令
```shell
$ sudo cat /etc/sudoers   # 查詢資訊
$ members sudo            # 查詢 sudo 群組資訊（在某些發行版非原生指令）
# 替代方案
$ getent group [group_name]
$ grep [group_name] /etc/group
$ sudo passwd root        # 修改 root 密碼
$ su -                    # 切換到 root 帳號
$ exit                    # 退出使用者
```

### 系統使用者
- 用於運行系統服務（如 Apache、Nginx、MySQL、Git 等）的特殊使用者。
- 通常無法登入系統，僅用於執行背景進程。

### 一般使用者
- 只擁有部分許可權，擁有唯一的用戶名和主目錄（位於 `/home/<用戶名>`）。
- 可以創建和管理自己的檔案和目錄，但無法執行影響系統整體的操作。

---

## 檔案權限

### 權限類型
Linux 檔案權限分為：
- **讀取 (Read)**：符號 `r`，數字 `4`。
- **寫入 (Write)**：符號 `w`，數字 `2`。
- **執行 (Execute)**：符號 `x`，數字 `1`。
- 無權限：符號 `-`，數字 `0`。

### 符號表示法
使用 10 個字元表示權限，例如：
```shell
-rwxr-xr-x
```
- 第一個字元：檔案類型，`-` 表示普通檔案，`d` 表示目錄，`l` 表示符號連結。
- 其餘九個字元分為三組，分別表示檔案所有者、所屬群組和其他用戶的權限。

### 數字表示法
使用三位數字表示權限，每位數分別對應擁有者、群組和其他使用者的權限總和，例如：
```shell
$ chmod 755 file.txt
```
表示：
- 擁有者：`7` = 4（讀取）+ 2（寫入）+ 1（執行）。
- 群組：`5` = 4（讀取）+ 1（執行）。
- 其他使用者：`5` = 4（讀取）+ 1（執行）。

### 變更權限
```shell
$ chmod [選項] 模式 檔案或目錄
```
- `-R` 或 `--recursive`：遞迴更改目錄及其內容的權限。

#### 範例
- 添加擁有者的執行權限：
  ```shell
  $ chmod u+x file.txt
  ```
- 所屬群組及其他用戶移除寫入權限：
  ```shell
  $ chmod go-w file.txt
  ```
- 遞迴更改目錄及其內容的權限：
  ```shell
  $ chmod -R 755 mydir
  ```

---

## 群組管理

### 群組概念
- Linux 使用群組管理使用者。預設情況下，使用者名稱為其主要群組名稱。
- 使用者可同時屬於多個群組，主要群組具有最高優先級。

### 常用指令

#### 查看使用者所屬群組
```shell
$ groups [用戶名]
```

#### 查看使用者的詳細資訊
顯示 UID、GID、主要群組及附加群組：
```shell
$ id [用戶名]
```

#### 創建與刪除群組
- 創建新群組：
  ```shell
  $ sudo groupadd 群組名
  ```
- 刪除群組：
  ```shell
  $ sudo groupdel 群組名
  ```

#### 更改檔案群組
- 更改檔案或目錄的群組：
  ```shell
  $ sudo chgrp 群組名 檔名
  ```
- 同時更改擁有者與群組：
  ```shell
  $ sudo chown 擁有者:群組 檔名
  ```
- 遞迴更改目錄：
  ```shell
  $ sudo chown -R 擁有者:群組 mydir
  ```

---

## 使用者管理

### 常用指令

#### 新建與刪除使用者
- 新建使用者：
  ```shell
  $ sudo adduser 用戶名
  ```
- 刪除使用者：
  ```shell
  $ sudo deluser 用戶名
  ```

#### 修改使用者屬性
- 更改用戶名：
  ```shell
  $ sudo usermod -l 新用戶名 舊用戶名
  ```
- 更改主目錄：
  ```shell
  $ sudo usermod -d 新目錄 用戶名
  ```
- 更改所屬群組：
  ```shell
  $ sudo usermod -g 群組名 用戶名
  ```
- 添加到附加群組：
  ```shell
  $ sudo usermod -aG 群組名 用戶名
  ```
- 鎖定用戶：
  ```shell
  $ sudo usermod -L 用戶名
  ```
- 解鎖用戶：
  ```shell
  $ sudo usermod -U 用戶名
  ```

#### 切換與查詢使用者
- 切換使用者：
  ```shell
  $ su 用戶名
  ```
- 查看當前使用者名稱：
  ```shell
  $ whoami
  ```
- 查看最近登錄清單：
  ```shell
  $ last
  ```
- 查看使用者資訊：
  ```shell
  $ finger 用戶名
  ```

---

## 系統管理

### 常用系統命令

#### 關閉與重啟系統
- 立即關閉：
  ```shell
  $ sudo shutdown -h now
  ```
- 定時關閉：
  ```shell
  $ sudo shutdown -h 23:00
  ```
- 重啟系統：
  ```shell
  $ sudo reboot
  ```

#### 系統狀態與網路
- 即時查看系統狀態：
  ```shell
  $ top
  ```
- 查看網路介面：
  ```shell
  $ ifconfig
  ```
- 檢查磁碟使用情況：
  ```shell
  $ df -h
  ```
- 查看網路連接與路由表：
  ```shell
  $ netstat
  ```
- 查看系統訊息（內核版本等）：
  ```shell
  $ uname -a
  ```

---

## 補充說明

### 編輯軟體操作
nano 是一個簡單易用的文字編輯器，可在終端中輕鬆編輯文字檔。
> 若需要更高級的編輯功能可考慮 vim 或 emacs
- `nano [filename]`：打開編輯器
- `Ctrl + O`：保存文件
- `Ctrl + X`：退出編輯器
- `Ctrl + [N | P | B | F]`：向下/上/左/右移動一個字元
- `Ctrl + Shift + 方向鍵`：選擇文本範圍
- `Ctrl + K`：複製選擇的文本
- `Ctrl + U`：粘貼文本
- `Ctrl + 6`：上一步
- `Alt + 6`：恢復上一步
- `Ctrl + W`：查找文本
- `Ctrl + \`：替換文本
- `Ctrl + G`：幫助

### /etc/passwd
/etc/passwd 是重要的系統檔，儲存系統使用者的基本資訊，此檔位於大多數 Unix 和 Linux 系統的根目錄，主要用於驗證用戶身份，並確認用戶的默認設置。
- 權限：所有使用者可讀，但大多數用戶不可寫。
- 密碼安全：密碼不存在此檔中，通常在 /etc/shadow 中，該檔僅系統管理員可見
- 用戶管理：編輯 /etc/passwd，管理員可添加、刪除或更改使用者基本資訊
- 系統功能：此檔會包含一些系統使用者，這些使用者用於運行系統服務與進程，通常沒有登錄權限，shell 欄位通常為 /sbin/nologin，表示不能登錄到系統

### 文件格式
```txt
username:password:UID:GID:GECOS:homeDir:shell
```
- username：用戶名
- password：密碼雜湊值，非實際密碼
- UID：用戶 ID
- GID：主要群組 ID
- homeDir：使用者主目錄，登陸後預設目錄
- shell：默認 shell，為用戶登錄後的命令列介面

---

## 實戰範例

### 檔案與目錄權限
1. **檢查檔案權限**：
   ```bash
   $ ls -l file.txt
   ```
2. **更改目錄權限**：
   ```bash
   $ chmod 755 mydir
   ```
3. **更改檔案擁有者與群組**：
   ```bash
   $ sudo chown user2:staff file.txt
   ```

### 使用者與群組管理
1. **新增使用者並設置密碼**：
   ```bash
   $ sudo adduser alice
   $ sudo passwd alice
   ```
2. **創建群組並添加使用者**：
   ```bash
   $ sudo groupadd devs
   $ sudo usermod -aG devs alice
   ```
3. **檢查使用者的群組**：
   ```bash
   $ groups alice
   ```
