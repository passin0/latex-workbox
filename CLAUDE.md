# 數位系統實驗報告（U2296 Digital System Laboratory）LaTeX 工作夾

這個資料夾放的是每週實驗報告的 LaTeX 原始檔。之後每次都是**新的一份報告**；舊資料夾只當範例參考，**不要修改**。

## 資料夾慣例

- 每份報告一個資料夾，命名為 `latex - <月日>`，例如 `latex - 918`、`latex - 1002`、`latex - 1016`（日期不補零）。
- 新報告以最新的結構（`latex - 1016`）為基準：
  ```
  latex - MMDD/
  ├── main.tex          # 只放前言 (preamble) + \include 各章
  ├── report.sty        # 共用樣式，從前一份直接複製，不要改
  ├── chapter/
  │   ├── goal.tex
  │   ├── theory.tex
  │   ├── instruments.tex
  │   ├── procedure.tex
  │   ├── results.tex
  │   └── problems.tex
  └── pic/              # 所有圖片（檔名可含空白，例如 `and gate.png`）
  ```
- `templete/` 是另一套較完整的論文／學期報告模板（`template.cls`、biblatex、tikz），實驗報告**不用**它，除非我另外要求。
- `temp.tex` 是「左圖右文」排版片段，可參考。
- `.aux .log .xdv .synctex.gz .fdb_latexmk .fls .out .bbl` 都是編譯產物，不用讀也不用改。

## 編譯

- 引擎：**XeLaTeX**（MiKTeX，Windows）。`report.sty` 用 `xeCJK`，pdfLaTeX 會失敗。
- 在報告資料夾內執行：`latexmk -xelatex main.tex`（或 `xelatex main.tex` 跑兩次，讓 `LastPage` 與交叉引用正確）。
- 字型：中文 `標楷體`，英文 `Times New Roman`（系統內建字型）。

## `report.sty` 內容摘要（共用，勿改）

xeCJK 中文設定、mathtools、graphicx、hyperref（無框線）、geometry（A4、邊界 2cm）、indentfirst + 2em 縮排、段距、setspace、changepage、tabularx、enumitem；把圖表名稱改成中文（`圖~`、`表~`）；用 fancyhdr 設定右下角頁碼「共 N 頁 第 n 頁」。

## `main.tex` 前言（照抄）

```latex
\documentclass[a4paper, 12pt]{article}

\usepackage{report}
\usepackage{tabularx}
\usepackage{algorithm}      % 也會帶入 float 套件，讓 [H] 可用
\usepackage{algpseudocode}
\usepackage{graphicx}
\usepackage{amsmath}
\usepackage{pdfpages}
\usepackage{caption}        % 提供 \captionof
\usepackage{amssymb}
\usepackage{listings}
\usepackage[most]{tcolorbox}
\usepackage[table]{xcolor}
\usepackage{multirow}
\usepackage{booktabs}

\pagestyle{plain}   % 注意：這行會蓋掉 report.sty 的「共 N 頁 第 n 頁」頁腳，見下方「已知問題」

\title{\textbf{U2296 Digital System Laboratory}\\ \Large LAB-XX: <實驗名稱>}
\author{<作者>}
\date{}

\begin{document}
\maketitle

\include{chapter/goal}
\include{chapter/theory}
\include{chapter/instruments}
\include{chapter/procedure}
\include{chapter/results}
\include{chapter/problems}

\end{document}
```

## 報告章節架構與寫法

- 報告內文用**英文**（依講義），程式碼註解可用中文。
- 章節一律用**不編號**的 `\section*{N. Title}`，手動寫編號；小節用 `\subsection*{...}`。
- 固定順序：

| # | 檔案 | 標題 | 內容 |
|---|---|---|---|
| 1 | `goal.tex` | `1. Goal` | 一兩句實驗目的（依講義） |
| 2 | `theory.tex` | `2. Theory` | 原理說明；常直接放講義截圖 `pic/1.png`（`width=0.8\textwidth`） |
| 3 | `instruments.tex` | `3. Instruments and Components` | booktabs + tabularx 的兩欄表：元件／數量 |
| 4 | `procedure.tex` | `4. Procedure` | `enumerate[leftmargin=*]` 步驟，可巢狀 `itemize`；結尾 `\newpage` |
| 5 | `results.tex` | `5. Results Summary` | 每個子實驗一個 `\subsection*`，左電路圖、右結果表 |
| 6 | `problems.tex` | `6. Problems and Discussion` | 遇到的問題與討論，之後接 `Student Feedback (Lab-XX)` 四題 |

（LAB-01 / `latex - 918` 是較早的版本：沒有 `\maketitle`，用置中大字標題，章節標題後面有冒號，Procedure 用 `quote` + `\item[Step n:]`。新報告不要用這種寫法。）

### 常用片段

**元件表**
```latex
\begin{table}[H]
\centering
\small
\begin{tabularx}{0.8\textwidth}{X r}
\toprule
\textbf{Instrument / Component} & \textbf{Quantity} \\
\midrule
DC Power Supply & 1 \\
IC: CD4081 (AND gate) & 1 \\
Connecting probes and wires & As required \\
\bottomrule
\end{tabularx}
\end{table}
```

**結果：左圖右真值表**（新報告用這個修正版，不要在 minipage 裡再包 `table` 環境）
```latex
\subsection*{AND Gate}
\begin{figure}[H]
    \centering
    \begin{minipage}[t]{0.48\textwidth}
        \vspace{0pt}
        \centering
        \includegraphics[width=\linewidth]{pic/and gate.png}
        \captionof{figure}{AND gate}
        \label{fig:and}
    \end{minipage}
    \hfill
    \begin{minipage}[t]{0.48\textwidth}
        \vspace{0pt}
        \centering
        \small
        \begin{tabularx}{\linewidth}{c c | >{\centering\arraybackslash}X | >{\centering\arraybackslash}X}
        \toprule
        \textbf{In A} & \textbf{In B} & \textbf{O/P (0/1)} & \textbf{O/P (V)} \\
        \midrule
        0 & 0 & & \\
        0 & 1 & & \\
        1 & 0 & & \\
        1 & 1 & & \\
        \bottomrule
        \end{tabularx}
        \captionof{table}{AND gate truth table}
    \end{minipage}
\end{figure}
```

**Student Feedback（每份報告結尾都要有）**
```latex
\section*{Student Feedback (Lab-XX)}
\textit{The last part of the lab requests your feedback. We are continually trying to improve the laboratory exercises to enhance your learning experience, and we are unable to do so without your feedback. Please include the following post-lab deliverables in your lab write-up.}

\begin{enumerate}[leftmargin=*]
    \item What did you like most about the lab assignment and why?
    \item What did you like least about it and why?
    \item Was there any section of the lab handout that was unclear? If so, what was unclear? Do you have any suggestions for improving the clarity?
    \item What suggestions do you have to improve the overall lab assignment?
\end{enumerate}
```

## 寫作規則

- 數學與單位：`$+5\text{V}$`、`$V_{DD} / V_{CC}$`、`$I = V/R$`、`$\rightarrow$`。
- 表格一律用 booktabs（`\toprule / \midrule / \bottomrule`），表內數值欄置中。
- 圖片放 `pic/`，路徑寫 `pic/<檔名>`（`report.sty` 的 `\graphicspath{figures/}` 沒有用到）。
- 需要固定位置的圖表用 `[H]`。
- 量測數據不得自行編造：沒提供的數值留空，等我補。
- 複製區塊時，記得同步改 `\subsection*` 標題、`\captionof` 文字和 `\label`（舊報告曾漏改）。

## 舊報告的已知問題（新報告要避免）

1. `\pagestyle{plain}` 放在 `\usepackage{report}` 之後，蓋掉了 fancyhdr 的「共 N 頁 第 n 頁」。新報告除非我另外指定，否則保留 `report.sty` 的頁腳（拿掉 `\pagestyle{plain}`）。
2. `figure` 裡的 minipage 又包了 `\begin{table}[H]`，形成浮動體包浮動體。改用上方的修正版片段。
3. `latex - 1016/chapter/results.tex`：OR gate 的小節標題寫成 AND Gate，AND／OR 的圖說都寫成 "not gate"。
4. `\author{Report Template}` 沒有改成作者名字。
