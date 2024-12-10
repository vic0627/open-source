# Shell 命令（二）

- [Shell 命令（二）](#shell-命令二)
  - [標準串流（Standard Streams）](#標準串流standard-streams)
    - [標準輸入（Standard Input, stdin）](#標準輸入standard-input-stdin)
    - [標準輸出（Standard Output, stdout）](#標準輸出standard-output-stdout)
    - [標準錯誤輸出（Standard Error, stderr）](#標準錯誤輸出standard-error-stderr)
  - [重新導向（Redirection）](#重新導向redirection)
    - [輸出重新導向](#輸出重新導向)
    - [輸入重新導向](#輸入重新導向)
    - [標準錯誤重新導向](#標準錯誤重新導向)
  - [管線（Pipeline）](#管線pipeline)
    - [基本概念](#基本概念)
    - [範例](#範例)
    - [管線內部機制](#管線內部機制)
    - [優點](#優點)
    - [限制](#限制)
  - [連結檔案（Links）](#連結檔案links)
    - [硬連結（Hard Link）](#硬連結hard-link)
    - [符號連結（Symbolic Link）](#符號連結symbolic-link)
  - [正規表示式](#正規表示式)
    - [基本正規表達式（Basic Regular Expression, BRE）](#基本正規表達式basic-regular-expression-bre)
    - [擴展正規表達式（Extended Regular Expression, ERE）](#擴展正規表達式extended-regular-expression-ere)
    - [常用符號](#常用符號)
  - [封存與壓縮](#封存與壓縮)
    - [封存（Archiving）](#封存archiving)
    - [壓縮（Compression）](#壓縮compression)
    - [封存與壓縮的組合](#封存與壓縮的組合)


## 標準串流（Standard Streams）

指程式與作業系統間進行資料交換的三個主要通道，會在程式執行時自動建立，並由以下三個檔案描述子（File Descriptor, FD）為代表：

### 標準輸入（Standard Input, stdin）

- 描述子 `0`。
- 提供程式讀取輸入資料的通道，預設來源是鍵盤。
- 使用者可以在互動式程式中輸入資料。

### 標準輸出（Standard Output, stdout）

- 描述子 `1`。
- 用於輸出程式的正常運作結果，預設輸出目標是終端機。
- 顯示程式的執行結果。

### 標準錯誤輸出（Standard Error, stderr）

- 描述子 `1`。
- 用於輸出錯誤訊息，預設目標也是終端機。
- 將錯誤訊息與正常輸出區分開。
- 可以獨立重新導向以便單獨記錄錯誤。

## 重新導向（Redirection）

一般來說，指令執行時需要資料的輸入以及執行結果的輸出：

- 鍵盤輸入的模式稱為*預設標準輸入*
- 螢幕輸出的模式稱為*預設標準輸出*

變更標準輸入或標準輸出的方式稱為**重新導向**。

### 輸出重新導向

- 基本輸出重新導向

    將命令的標準輸出重新導向到文件，會覆蓋文件內容，若文件不存在則創建。

    ```shell
    command > file
    ```

    範例：

    ```shell
    # 將目錄查詢結果轉存到 file.txt
    ls > file.txt
    ```

- 追加輸出重新導向

    將標準輸出追加到文件末尾，而不是覆蓋。

    ```shell
    command >> file
    ```

    範例：

    ```shell
    # 將目錄查詢結加到 file.txt 末尾
    ls >> file.txt
    ```

### 輸入重新導向

- 基本輸入重新導向

    從文件而不是標準輸入（鍵盤）讀取數據。

    > 有時可省略此符號

    ```shell
    command < file
    ```

    範例：

    ```shell
    # 將 file.txt 的內容作為 wc 的輸入計算 file.txt 的行數
    wc -l < file.txt
    ```

### 標準錯誤重新導向

- 單獨重新導向標準錯誤

    將標準錯誤信息重新導向到文件。

    ```shell
    command 2> file
    ```

    範例：

    ```shell
    ls file_not_exist 2> error.log
    ```

- 追加重新導向標準錯誤

    將標準錯誤信息加到文件。

    ```shell
    command 2>> file
    ```

    範例：

    ```shell
    ls file_not_exist 2> error.log
    ```

## 管線（Pipeline）

通過連接多個指令的標準輸出和標準輸入，實現資料流的處理機制，允許用戶將一個指令的輸出作為另一個指令的輸入。

### 基本概念

1. 資料流：資料由一個指令的標準輸出流向下一個指令的標準輸入，無需存到中間文件。
2. 串連執行：各指令按順序執行，首個指令執行完成部分輸出後，這些資料立即進入下一個指令處理，直到整個資料流處理完成。
3. 靈活組合：可通過管道將多個指令組合成複雜的工作流程，每個指令專注於完成一個具體的任務。

語法：

```shell
command1 | command2 | command3
```

### 範例

1. 查看當前目錄中的 .txt 文件

    ```shell
    ls | grep '.txt'
    ```

2. 統計目錄中 .txt 文件數量

    ```shell
    ls | grep '.txt' | wc -l
    ```

3. 查找大文件

    ```shell
    du -h | sort -h | tail -n 10
    ```

### 管線內部機制

1. 標準輸入和輸出：
   - UNIX 的管線機制依賴於將一個指令的 stdout 傳遞給下一個指令的 stdin。
2. 管道檔案描述子：
   - 系統通過建立一個臨時的管道，實現兩指令間的資料傳遞。
   - 使用 pipe() 系統呼叫創建的兩個檔案描述子：一個用於讀取，另一個用於寫入。
3. 並行執行：
   - 通常管線中的指令以並行方式執行，不是等一個指令執行完才執行下一個指令。
   - 每個指令的執行會停留在輸入或輸出操作上，直到有可用的資料。

### 優點

1. 高效率，不需將中間結果寫入檔案，直接在記憶體中傳遞。
2. 高靈活度，透過組合多個小工具，可以完成複雜的任務。
3. 簡化操作，通過組合指令，無需手動處理中間文件或步驟。

### 限制

1. 管線通常用於處理純文本數據，要處理二進制數據需要額外的工具。
2. 管線中出現錯誤時，難以定位是哪個指令導致。
3. 並行可能會導致某些情況下會因為輸入或輸出阻塞而降低效率。

## 連結檔案（Links）

連結檔（Links）是一種特殊的檔案，它們指向另一個檔案或目錄。在 Linux 中，有兩種主要類型的連結檔：硬連結和符號連結（也稱為軟連結或符號連接）。硬連結直接指向磁碟上的相同數據塊，而符號連結則包含指向目標檔案或目錄的路徑。

### 硬連結（Hard Link）

- 本質：硬連結是指向檔案**數據的真實位置（inode）**的一個指標。多個硬連結實際上是對同一個檔案的引用。

> 使用 `ls -i [FILE]` 可以查詢檔案 inode。

- 共享 inode：硬連結與原始檔案共享相同的 inode 編號，表示他們指向同一塊數據。
- 同步更新：如果對任一硬連結進行修改，其他硬連結的內容也會同步變更，因為它們指向同一塊數據。
- 限制：
  1. 只能用於檔案，不能用於目錄。
  2. 必須位於同一個檔案系統中（不能跨分區）。
- 刪除行為：刪除某個硬連結不會刪除數據，數據只有在所有硬連結都刪除後才會被移除。
- 應用場景：
  1. 用於需要共享數據而不擔心檔案刪除的情況。
  2. 節省儲存空間，避免數據冗余。
- 範例：
  
  ```shell
  ln original.txt hardlink.txt
  ```

  建立一個硬連結 `hardlink.txt`，它與 `original.txt` 共享數據。

### 符號連結（Symbolic Link）

- 本質：符號連結是指向檔案名稱的「捷徑」或「別名」，它儲存的是原始檔案的路徑，而非數據的實際位置。
- 不同 inode：符號連結擁有自己獨立的 inode，與原始檔案 inode 不同。
- 靈活性：可用於檔案和目錄，並可跨分區創建。
- 斷鏈問題：如果原始檔案被刪除或移動，符號連結會失效（斷鏈）。
- 刪除行為：刪除符號連結只會刪除連結本身，不會影響原始檔案。
- 應用場景：
  1. 用於建立跨目錄或跨分區的快捷方式。
  2. 常見於配置檔案（例如 `/etc` 下）或目錄別名。
- 範例：

  ```shell
  ln -s original.txt symlink.txt
  ```

  建立一個符號連結 `symlink.txt`，指向 `original.txt`。

## 正規表示式

「正規表示式」是用於文本搜索與處理的一種強大工具，它可以通過模式匹配來查找、替換或分析字串。

### 基本正規表達式（Basic Regular Expression, BRE）

- 是 POSIX 標準的一部分，適用 `grep`（預設）、`sed`、`awk` 等。
- 不支援某些更複雜的模式，或者需要額外使用反斜線來啟用特殊功能。
- 符號如 `+`、`?`、`|` 等在 BRE 中需要使用 `\` 進行轉義才能啟用其特殊功能。

### 擴展正規表達式（Extended Regular Expression, ERE）

- 是 BRE 的超集，增加更多功能，並簡化了一些符號的使用。
- 無需轉義即可使用符號如 `+`、`?`、`|` 等。
- 適用 `egrep` 或 `grep -E`，`awk` 的正規表示式語法也基於 ERE。

### 常用符號

| BRE       | ERE     | 含義                                         |
| --------- | ------- | -------------------------------------------- |
| `.`       | `.`     | 匹配任意單個字元（不包括換行符）             |
| `^`       | `^`     | 匹配行的開頭                                 |
| `$`       | `$`     | 匹配行的結尾                                 |
| `*`       | `*`     | 匹配前一個字元或模式零次或多次               |
| `\+`      | `+`     | 匹配前一個字元或模式一次或多次               |
| `\?`      | `?`     | 匹配前一個字元或模式零次或一次               |
| `\\|`     | `\|`    | 匹配「或」操作的模式                         |
| `[]`      | `[]`    | 定義字符集，匹配方括號內任意字元             |
| `[^]`     | `[^]`   | 定義否定字符集，匹配不在括號內的任意一個字元 |
| `\{n\}`   | `{n}`   | 匹配前一個模式 n 次                          |
| `\{n,m\}` | `{n,m}` | 匹配前一個模式至少 n 次，至多 m 次           |

> ERE 中，若要將特殊符號作為匹配的字元，可使用 `\` 保留該符號的字面值。

範例：

```shell
echo 'axb aab a2c' | grep -o 'a.b'
# 輸出
# axb
# aab

echo 'abc\ncba\naba\nbad' | grep '^ab' 
# 輸出
# abc
# aba

echo 'abc\ncbc\naba\nbad' | grep 'bc$' 
# 輸出
# abc
# cbc

echo 'ac\nabc\nabbc\nadc' | grep 'ab*c'
# 輸出
# ac
# abc
# abbc

echo 'ac\nabc\nabbc\nadc' | grep -E 'ab+c'
# 輸出
# ac
# abc

echo 'ac\nabc\nabbc\nadc' | grep -E 'abc|adc'
# 輸出
# abc
# adc

echo 'ac\nabc\nabbc\nadc' | grep -E 'a[bd]c'
# 輸出
# abc
# adc

echo 'ac\nabc\nabbc\nadc' | grep -E 'a[^b]c'
# 輸出
# adc

echo 'a\naa\naaa\naaaa\nacaaa' | grep -E 'a{2,4}'
# 輸出
# aa
# aaa
# aaaa
```

## 封存與壓縮

### 封存（Archiving）

將多個文件和目錄打包成單一文件的過程，目的是便於儲存和管理，但文件的大小不會減少。

常用工具：

- `tar`：常見的封存工具，用於將多個文/目錄合併為一個 `.tar` 文件。

範例：

1. 創建封存文件

    ```shell
    tar -cf archive.tar file1 file2 dir1
    ```

    - `-c`：創建新的封存文件
    - `-f`：指定封存文件名稱
  
2. 解封存

    ```shell
    tar -xf archive.tar
    ```

    - `-x`：提取封存內容

3. 檢視封存內容

    ```shell
    tar -tf archive.tar
    ```

    - `-t`：列出封存文件中的內容

### 壓縮（Compression）

將文件的大小減少的過程，目的是節省儲存空間和加速傳輸，通常使用壓縮算法減少文件體積。

常用工具：

- `gzip`：壓縮文件生成 `.gz` 文件
- `bzip2`：壓縮文件生成 `.bz2` 文件
- `xz`：壓縮文件生成 `.xz` 文件

範例：

1. 壓縮文件

    ```shell
    gzip file.txt
    ```

    將 `file.txt` 壓縮為 `file.txt.gz`。

2. 解壓縮文件

    ```shell
    gunzip file.txt.gz
    ```

    解壓縮生成原始的 `file.txt`。

### 封存與壓縮的組合

1. 封存並壓縮

    ```shell
    tar -czf archive.tar.gz file1 file2 dir1
    ```

    - `-c`：創建封存
    - `-z`：使用 gzip 壓縮
    - `-f`：指定文件名稱

2. 解壓縮並解封存

    ```shell
    tar -xzf archive.tar.gz
    ```

    - `-x`：提取封存內容
    - `-z`：使用 gzip 解壓縮