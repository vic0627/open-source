# Shell Script 程式設計

## 基本語法

### 認識 Shell Script

- **Shell Script** 是由一系列的 shell 命令組成的腳本，包含動作、判斷、迴圈等。
- 簡單的直譯程式語言，無需編譯，直接執行。

### 如何執行 Shell Script 程式

1. **在電腦本地端撰寫與執行**
   - 使用文字編輯器撰寫腳本文件。
   - 在文件開頭宣告命令解析器，例如：`#!/bin/bash`。
   - 確保腳本有執行權限，使用命令：`chmod +x script.sh`。
   - 執行腳本：
     - 使用路徑：`./script.sh`
     - 使用解析器：`bash script.sh` 或 `sh script.sh`
     - 使用 `source`：`source script.sh`
2. **在 Google Colab 環境撰寫與執行**
   - 在單元格撰寫 Shell Script 程式並執行。

### 腳本變數

- 變數不需定義資料型態。
- 變數區分大小寫。
- 定義變數時，`=` 兩側不可有空格。
- 語句結尾無需分號。
- 取值需在變數前加 `$`。

### 特殊變數

- `$0`：腳本名稱
- `$1, $2, ...`：傳遞給腳本的參數
- `$#`：參數數量
- `$@` 和 `$*`：所有參數
- `$$`：當前 Shell 的進程 ID
- `$?`：上一條命令的退出狀態碼

## 運算子

### 算術運算子

- **基本運算**
  - 加法：`+`
  - 減法：`-`
  - 乘法：`*`
  - 除法：`/`
  - 取餘：`%`
- **用法**
  - `expr`：外部指令，需使用反引號或 `$()`。
  - `$(( ))`：內建運算，推薦使用。
  - `let`：內建命令，簡化語法。

### 比較運算符

- **數值比較**：`-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`
- **字串比較**：`=`, `!=`, `<`, `>`

### 邏輯運算子

- 邏輯與：`&&`
- 邏輯或：`||`
- 邏輯非：`!`

### 賦值運算子

- 賦值：`=`
- 複合賦值：`+=`, `-=`, `*=`

### 檔案屬性運算子

- 檢查文件或目錄屬性：
  - 是否存在：`-e`
  - 是否為普通文件：`-f`
  - 是否為目錄：`-d`
  - 是否可讀：`-r`
  - 是否可寫：`-w`
  - 是否可執行：`-x`
  - 是否非空：`-s`

## 條件檢查與 `test` 用法

### 基本語法

- `test EXPRESSION` 或 `[ EXPRESSION ]`
- 當條件為真時返回 `0`，否則返回非零值。

### 常見用法

- **檔案檢查**
  ```bash
  [ -e file ]  # 檢查文件是否存在
  ```
- **數值比較**
  ```bash
  [ $a -eq $b ]  # 檢查是否相等
  ```
- **字串比較**
  ```bash
  [ "$a" = "$b" ]  # 檢查字串是否相等
  ```
- **邏輯運算**
  ```bash
  [ $a -eq 1 ] && [ $b -eq 2 ]  # 邏輯與
  ```

## 流程控制

### 條件語句

- **if 語句**
  ```bash
  if [ condition ]; then
      commands
  elif [ condition ]; then
      commands
  else
      commands
  fi
  ```
- **case 語句**
  ```bash
  case $variable in
      pattern1)
          commands
          ;;
      pattern2)
          commands
          ;;
      *)
          default_commands
          ;;
  esac
  ```

### 迴圈語句

- **for 迴圈**

  ```bash
  for variable in list; do
      commands
  done

  for (( i=0; i<10; i++ )); do
      commands
  done
  ```

- **while 迴圈**
  ```bash
  while [ condition ]; do
      commands
  done
  ```
- **until 迴圈**
  ```bash
  until [ condition ]; do
      commands
  done
  ```
- **控制迴圈**
  - `break`：終止整個迴圈。
  - `continue`：跳過當前迴圈，進入下一次執行。

### 雙和號 (&&) 與雙豎線 (||)

- **範例**
  ```bash
  [ -f /etc/passwd ] && echo "passwd is file!"
  [ ! -f /etc/passwd ] || echo "passwd is file!"
  ```

## 命令列參數

### 認識命令列參數

- **命令列參數** 讓腳本在執行時更靈活，處理用戶輸入或提供檔案名稱、數據。
- 使用特殊變數訪問參數：
  - `$0`：腳本名稱。
  - `$1, $2, $3, ...`：第一個、第二個、第三個參數，以此類推。
  - `$#`：參數總數。
  - `$*`：所有參數作為單個字串。
  - `$@`：所有參數作為個別字串。
  - `$$`：當前腳本的進程 ID。

### 範例

```bash
# params.sh
#!/bin/bash
echo "腳本名稱：$0"
echo "第一個參數：$1"
echo "所有參數：$*"
```

- 執行：`bash params.sh arg1 arg2`

## 特殊字元

### 常見特殊字元

- `*`：匹配任意字元。
- `?`：匹配單一字元。
- `\`：跳脫字元。
- `'`：單引號，維持內部字元原樣。
- `"`：雙引號，部分特殊字元如 `$` 仍可解釋。

### 範例

```bash
echo "* 處理範例"
echo "跳脫範例：\n"
echo '單引號範例 $PATH'
echo "雙引號範例 $PATH"
```

## 函數的意義與宣告

### 函數的優勢

- **程式碼重用**：避免重複。
- **提高可讀性**：分離功能模組。
- **便於維護**：集中邏輯修改。

### 宣告與使用

```bash
function_name() {
  # 函數內的程式碼
}

# 或
function function_name() {
  # 函數內的程式碼
}
```

### 範例

```bash
calculate_sum() {
  echo "結果：$(( $1 + $2 ))"
}

calculate_sum 5 10
```

## 檔案讀取與寫入

### 檔案讀取

- **使用 cat**：一次性讀取整個文件。
- **使用 while 迴圈**：逐行讀取。

### 檔案寫入

- **輸出導向**：`>` 和 `>>` 寫入或附加到檔案。
- **Here Document**：多行文本寫入。

### 範例

```bash
# 讀取文件
cat file.txt

# Here Document 寫入
cat << EOF > file.txt
Line 1
Line 2
EOF
```

## 字串操作

### 字串擷取

- 截取部分字串：
  ```bash
  substring=${my_string:1:6}
  ```
- 從開頭或末尾截取：
  ```bash
  substring=${my_string:0:5}
  substring=${my_string: -5}
  ```
- 使用正規表達式：
  ```bash
  matched=$(echo "$string" | grep -oE "pattern")
  ```

### 字串分割

- 使用 `cut` 或 `awk`：
  ```bash
  echo "$string" | cut -d ',' -f 1
  ```
- 使用 IFS：
  ```bash
  IFS="," read -ra array <<< "$string"
  ```

### 範例

```bash
original_string="apple,orange,banana"
IFS="," read -ra parts <<< "$original_string"
echo "第一個部分：${parts[0]}"
```

## 綜合範例

### 題目：數列計算

- 使用命令列參數顯示數字序列。

```bash
#!/bin/bash
n=$1
for i in $(seq 1 $n); do
  echo $i
done
```

### 題目：函數實作

- 計算兩數和。

```bash
#!/bin/bash
calculate_sum() {
  echo "Sum: $(( $1 + $2 ))"
}

calculate_sum 10 20
```

### 題目：讀取與分割

- 從文件讀取資料並分割。

```bash
cat << EOF > songs.txt
song1,artist1
song2,artist2
EOF

while IFS="," read -r song artist; do
  echo "Song: $song, Artist: $artist"
done < songs.txt
```
