# 使用 Shell 的實用功能

- [使用 Shell 的實用功能](#使用-shell-的實用功能)
  - [機制](#機制)
    - [功能](#功能)
  - [萬用字元](#萬用字元)
  - [指令提示](#指令提示)
  - [管理歷史命令](#管理歷史命令)
  - [別名](#別名)
    - [新增](#新增)
    - [刪除](#刪除)
  - [Shell 環境變數](#shell-環境變數)
    - [PS1](#ps1)
    - [PATH](#path)
    - [LANG](#lang)

## 機制

- 為 user 與 Linux（kernel，核心）之間的橋樑
- 為 Linux 的秘書，可接收各種工作，方便使用者進行各種作業
- 是作業系統的命令列解譯器，是使用者與作業系統內核之間的介面

Shell 與 Linux 的關係：

```txt
hardware => kernel => shell => user
```

### 功能

- 命令解釋和執行
- 文件系統導航
- 環境變數管理
- 程序控制
- 腳本編寫
- 重定向和管道
- 用戶許可權管理
- 系統配置
- 歷史紀錄管理
- 別名與函數

## 萬用字元

為檔案操作的搜索中常用的特殊字元，用來匹配多個字元或檔名。

- `*`：匹配 0 或多個字元。例如，`*.txt` 匹配任何以 .txt 結尾的檔名
- `?`：匹配一個單獨字元。例如，`file?.txt` 匹配 file1.txt、fileA.txt 等
- `[]`：指定字元集合，匹配其中任一字元。例如，`[aeiou]` 匹配任一母音字母
- `[-]`：指定字元範圍，匹配範圍內任意字元。例如，`[0-9]` 匹配 0 到 9 任意數字
- `[!]` 或 `[^]`：否定字元集合，匹配不在集合中的字元。例如，`[^aeiou]` 匹配任ㄧ不是母音字母的字元
- `{}`：創建模式清單，匹配清單中任意模式。例如，`{jpg,png,gif}`

## 指令提示

在輸入命令過程中按下 Tab 鍵。

例如：

```shell
ls RE[Tab]
```

會自動顯示 `ls README.md`。

如果 RE 開頭檔案有多個，可以多按幾次 Tab。

## 管理歷史命令

按方向鍵上或下可以重新顯示之前輸入過的命令。

使用 `history` 指令列出命令歷程清單，若長度太長，可用 `history | less`：

```shell
history | less
# ...
2610  ls
2611  ls doc
2612  ls README.md
2613  ls -l README.md
```

接著輸入 `!列編號` 可呼喚對應命令：

```shell
!2612
ls README.md
```

選項：

- `-d 列編號`：刪除特定歷史
- `-c`：清除所有歷史

## 別名

### 新增

```shell
alias 別名='命令'
```

### 刪除

```shell
unalias 別名
```

## Shell 環境變數

### PS1

要變更命令提示符可設定 PS1，PS1 用於定義命令列提示符的外觀和行為。

常見 PS1 變數表示法：

- `\u`：當前用戶名稱
- `\h`：主機名稱，僅顯示前綴
- `\H`：完整主機名稱
- `\w`：當前工作完整路徑
- `\W`：當前工作路徑的最後一個路徑
- `\t`：24 小時制時間（HH:MM）
- `\d`：日期（W M D）
- `\A`：24 小時制時間（HH）
- `\@`：12 小時制時間
- `\$`：根據用戶身份顯示 $（普通用戶） 或 #（root）

顯示 PS1 內容：

```shell
echo $PS1
```

設定 PS1：

```shell
PS1='\u@\h:\w\$'
```

### PATH

用於指定作業系統要在哪些目錄尋找可執行檔。

顯示 PATH 內容：

```shell
echo $PATH
```

> 顯示結果各個路徑以冒號分隔

新增路徑：

```shell
PATH="$PATH:絕對路徑"
# or
export PATH-$PATH:絕對路徑
```

### LANG

Linux 支援英、中及各種世界語言，並可隨時切換。

顯示 LANG 內容：

```shell
echo $LANG
```

修改 LANG：

```shell
export LANG=en_US.UTF-8
```
