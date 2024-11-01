# 開源軟體實務

## 開源軟體簡介

**開源軟體（Open Source Software, OSS）**是指其原始碼可以**公開獲取**、**使用**和**分發**的軟體。

- 原始碼公開：開源軟體的原始碼對所有人公開，意味著任何人都能訪問、學習和理解該軟體的內部運作。此公開性促進軟體的透明度與信任度，使用者能更清楚了解軟體的行為與功能。
- 自由使用：開源軟體通常依照開源授權協議（GPL、MIT、Apache 等）釋出，這些協議明確規定了使用者的權利義務。用戶可以**免費**使用這些軟體，無論個人、企業或政府機構，無需支付授權費。
- 允許修改：開源軟體允許使用者修改原始碼，這是開源的一大優勢。開發者能根據自己的需求修訂和優化，使軟體能夠適應不同使用場景。這種可以快速調整軟體來解決問題的靈活性，對於技術團隊來說特別有價值。
- 自由分發：開源軟體允許任何人分發修改後的版本。此特性促進了知識傳播與技術普及，且鼓勵更多貢獻者參與軟體的改進與擴展。

### 代表性開源軟體

- Linux：開源的操作系統**核心**，許多不同的**發行版**（Ubuntu、Fedora 等）都是基於 Linux 核心。
- Apache HTTP Server：最流行的 Web 伺服器軟體之一，用來提供網頁服務。
- Mozilla Firefox：開源的網頁瀏覽器，以安全性與擴展性著稱。
- LibreOffice：開源的辦公室軟體套件，包括文字處理、試算表、簡報等應用。

### 開源軟體特性

- 開放原始碼（Open Source）
- 自由使用權（Free to Use）
- 可修改性（Modifiability）
- 社群參與（Community Participation）
  - 開發者
  - 用戶
  - 貢獻者
  - 社群協作
- 跨平台支援（Cross-Platform Support）
- 可持續性（Sustainability）
  - 捐贈（Donations）
  - 商業支持（Commercial Sponsorship）
  - 付費支援（Paid Support Services）
  - 相關服務和產品（Related Services and Products）
  - 諮詢和整合服務（Consulting and Integration Services）
- 安全性（Security）
- 自由授權（Licensing）

### 開源許可證

規範軟體的使用、修改和分發的法律文件，以下是常見的開源許可證：

- GNU 通用公共許可證（GNU General Public License, GPL）
- MIT 許可證（MIT License）
- Apache 許可證（Apache License 2.0）
- BSD 許可證（Berkeley Software Distribution License）
- Mozilla 公共許可證（Mozilla Public License, MPL）

## Linux 簡介

軟體可分兩大類：

- 基本軟體（System Software）：通常在底層運行且不直接提供應用功能，負責管理、控制電腦硬體資源，協調和提供支援，以便其他軟體（含應用軟體）能夠運行和執行任務，例如作業系統、裝置驅動程式、系統工具和程式庫。
- 應用軟體（Application Software）：又稱應用程式或 APP，是用戶最常互動的軟體類型，旨在解決各種不同工作、娛樂和通訊需求，例如瀏覽器、遊戲、文字處理軟體或試算表等。

### 發展

- 早期 UNIX（1960s ~ 1970s）
    - 起初由 Bell Labs 的 Ken Thompson、Dennis Ritchie 等開發的實驗性操作系統，目標提供多人、多工處理的環境。
    - 後因法律問題，AT&T 將 UNIX 程式碼公開。
    - 1970 年代，UNIX 逐步在大學、研究所普及。Bill Joy，當時為加州大學柏克萊分校學生，帶領團隊催生出 UNIX 改良版的 BSD（Berkeley Software Distribution），打造網路環境。
    - BSD 系統包括多個版本，如 FreeBSD、OpenBSD、NetBSD 等。
- UNIX 的商業化（1980s）
    - 卡內基美隆大學教授 Andrew Tannenbaum 改良 UNIX 成為 Apple 的 macOS 雛形。
    - 隨 UNIX 成功，許多公司開始基於 UNIX 進行開發，並推出自己的版本，如 IBM 的 AIX、HP 的 HP-UX 和 Sun Microsystems 的 Solaris，這些商業版本在企業環境中取得了廣泛的應用。
- Linus Torvalds 的創建（1991）
    - Linux 的開發始於芬蘭大學生 Linus Torvalds，他於 1991 釋出了第一個 Linux 內核的版本。這是個人項目，但很快就吸引全球開發者與貢獻者。
- 開源運動的崛起（1990s ~ Present）
    - Linux 的成功促使了開源運動的發展，它強調軟體自由和共享。許多企業和社群參與到 Linux 的開發中，形成龐大生態系統。
- Linux 在不同領域的應用（Present）
    - 伺服器、超級電腦、嵌入式系統、雲運算和物聯網等，是多用途操作系統的代表。

>  Linux 是基於 UNIX 設計理念和哲學的開源作業系統，他們雖有不同起源和發展路徑，但在很大程度上共用相似的概念和原則。

> Linux 不是直接由 UNIX 核心修改而成，而是從零開發的**類 UNIX（Unix-like）**操作系統，因開發上沒有使用 UNIX 的原始碼，所以不是 UNIX 的直接衍生版本。

#### OS 的發展

| OS      | -                                        |
| ------- | ---------------------------------------- |
| macOS   | 改良 UNIX 成為 Apple 的 OS               |
| Linux   | 開源的 UNIX 系列 OS                      |
| OpenBSD | 開源的 UNIX 系列 OS                      |
| Android | Google 自 Linux 改良的手機專用 OS        |
| iOS     | Apple 公司自 macOS 改良的 iPhone 專用 OS |
| Solaris | Oracle 公司改良的 UNIX                   |

#### 伺服器 OS

> Server 與 Client 是在計算機網路環境中常見的兩種角色。

- 伺服器（Server）是一或多台計算機，接受用戶端請求並提供相應資源或服務，例如 Web Server（Apache、Nginx）、Mail Server（Exchange、Postfix）、File Server（FTP）和遊戲伺服器等。
- 用戶端（Client）是終端用戶所使用之設備，會向 Server 發送請求，並接收、處理伺服器返回的資訊，例如瀏覽器（Chrome、Firefox）、電郵客戶端（Outlook、Gmail）、文件傳輸客戶端（FTP Client）和遊戲用戶端等。

這種資料由 Server 提供，再由 Client 接收的架構稱為**主從式架構**，許多網路服務都屬於此架構。

Server OS 是專為運行各類伺服器應用程式而設計的操作系統，具備一些獨特的特徵：

- 支援網路大量存取
- 速度快
- 非常穩定可靠
- 沒有授權問題（e.g. 最高連線數目限制）
- 性價比極高（e.g. 可於一台電腦架設多個 Server）
- 容易維護
  
而 Linux 是符合上述條件的 Server OS，而且免費使用，普及於個人、企業、機構等。

Linux 發行版（Linux Distribution）通常區分以下兩種：

- GUI（Graphic User Interface）
- CUI（Character User Interface）

Linux 發行版可追朔至 90 年代初期，由個別開發人員與社群創建，旨在將 Linux 內核與開源軟體和工具整合到完整的操作系統中。

#### GNU

Richard Stallman 在 1980 年代創建 GNU（GNU's Not Unix）計畫，旨在開發一個完全自由和開放的 Unix-like 操作系統，但也強調它不是 Unix 的拷貝，而是受到 Unix 設計理念啟發，然而 GNU 計畫的內核 Hurd 進展緩慢。

由於 Hurd 進展緩慢，Linux 內核被選為替代方案，開發者開始整合 Linux 內核與 GNU 工具和應用程式，創建一個完整的自由 Unix-like 操作系統，這就是現代 Linux 發行版的起源。