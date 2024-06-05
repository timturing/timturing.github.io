## 图

注意无论图表，使用带`*`的一般是跨两列格式，不带的是单列格式，如`figure`和`figure*`的区别。

### $1 \times 1$

```latex
\begin{figure}[t]
    \centering
    \includegraphics[width=0.95\columnwidth]{images/neuron_number_increase.pdf}
    \caption{Change in PPL across different languages upon incremental number of language specific neurons when deactivating French neurons.}
    \label{fig-neuron-number}
\end{figure}
```

### $1 \times 2$

```latex

\begin{figure}[t]
    \begin{subfigure}{0.49\linewidth}
        \includegraphics[width=\linewidth]{images/Llama-2-70b-hf_cl.pdf}
        \caption{LLaMA-2 (70B)}
        \label{fig:cl-llama}
   \end{subfigure}
   \begin{subfigure}{0.49\linewidth}
        \includegraphics[width=\linewidth]{images/bloom_cl.pdf}
        \caption{BLOOM (170B)}
        \label{fig:cl-bloom}
   \end{subfigure}
\centering
\caption{Dominance scores (mean SES)  across layers when different languages serve as the target language.}
\label{fig:cl}
\end{figure}
```

### $2 \times2$

```latex
\begin{figure}[t!]
\centering
\begin{subfigure}{0.49\linewidth}
\includegraphics[width=\linewidth]{images/results0.pdf}
\caption{LAPE}
\end{subfigure}
\begin{subfigure}{0.49\linewidth}
\includegraphics[width=\linewidth]{images/results1.pdf}
\caption{LAVE}
\end{subfigure}
\begin{subfigure}{0.49\linewidth}
\includegraphics[width=\linewidth]{images/results2.pdf}
\caption{PV}
\end{subfigure}
\begin{subfigure}{0.49\linewidth}
\includegraphics[width=\linewidth]{images/results3.pdf}
\caption{RS}
\end{subfigure}
\caption{Impact of four identification methods on the PPL increase of LLaMA-2 (7B). The element at the $i$-th row and $j$-th column is the PPL change for language $j$ due to perturbations in a specific region of language~$i$.}
\label{fig:ppl-results1}
\end{figure}
```

## 表

**注意首行首列一般加粗，重点结果加粗**

### 多线表

增加中间线数量使用`\midrule`，增加列数使用`&`分割，增加行数使用`\\`换行即可。

```latex
\begin{table}[t]
\centering
\resizebox{1.0\columnwidth}{!}{
\begin{tabular}{ccccccc}
\toprule
   & \textbf{zh}   & \textbf{fr}   & \textbf{es}   & \textbf{vi}   & \textbf{id}   & \textbf{ja}   \\ 
\midrule
\textbf{normal} & \textbf{4.30} & \textbf{4.19} & \textbf{3.51} & \textbf{3.70} & \textbf{4.16} & \textbf{2.86} \\
\midrule
\textbf{zh} & 2.46 & 3.56 & 2.96 & 3.64 & 3.56 & 2.31 \\
\textbf{fr} & 3.69 & 2.50 & 2.29 & 3.01 & 3.59 & 2.76 \\
\textbf{es} & 3.51 & 2.57 & 2.01 & 3.14 & 3.34 & 2.56 \\
\textbf{vi} & 3.93 & 3.19 & 2.49 & 2.74 & 3.59 & 2.74 \\
\textbf{id} & 3.67 & 3.10 & 2.67 & 3.21 & 2.84 & 2.80 \\
\textbf{ja} & 3.21 & 3.69 & 3.07 & 3.49 & 3.37 & 1.84 \\ 
\bottomrule
\end{tabular}}
\caption{Performance of LLaMA-2 (70B) on the multilingual Vicuna as evaluated by GPT-4. The ``normal'' row is baseline scores without deactivation. Subsequent rows are scores with deactivation of specific neurons.
}
\label{tab:mvicuna}
\end{table}
```

### 其他语言夹杂表

```latex

\begin{table}[!t]
\small
\centering
\begin{tabular}{p{0.95\columnwidth}}
    \toprule
    \textbf{Question} \\
    \begin{CJK*}{UTF8}{gbsn} 你是一位登上珠穆朗玛峰顶峰的登山者。描述一下你的情绪和从高处看到的景色。\end{CJK*} \\
    \textcolor{gray}{(\textit{Translation}: You are a mountain climber reaching the summit of Mount Everest. Describe your emotions and the view.)} \\
    \midrule
    \textbf{Normal output} \\
    \begin{CJK*}{UTF8}{gbsn}我是一个登上珠穆朗玛峰顶峰的登山者。当我站在山顶时，我感到非常兴奋和自豪。…\end{CJK*} \\
    \textcolor{gray}{(\textit{Translation}: I am a climber who has reached the summit of Mount Everest. When I stood on the top of the mountain, I felt very excited and proud. ...)} \\
    \midrule
    \textbf{Deactivated output} \\
    \begin{CJK*}{UTF8}{bsmi}我是一個登上珠穆朗瑪峰頂峰的登山者。 I am a mountaineer who has climbed to the top of Mount Everest. 當我站在珠my朗ma峰頂峰，我感到非常興奮和欣慰。 … \end{CJK*}\\
    \bottomrule                                            
\end{tabular}
\caption{Illustration of LLaMA-2-70B responses to a question in Simplified Chinese. The text in black is model's actual output and \textcolor{gray}{text in gray} is our self-added translation. The deactivated output is the generation when neurons of Simplified Chinese are deactivated.}
\label{tab:example}
\end{table}
```

### 增加竖线 （在确定列的对齐方式处添加“|”）

```latex
\begin{table}[!t]
\centering
\small
\begin{tabular}{c|cc}
\toprule
\textbf{Proportion} & \multirow{2}{*}{\textbf{MMLU}} & \multirow{2}{*}{\textbf{Alpaca-Eval}} \\
\textbf{FLAN / Alpaca} &  &  \\
\midrule
100 / 0 & 50.6  & 15.0 \\
50 / 50 & 50.5  & 44.4  \\
0 / 100 & 47.5  & 47.2  \\
\cmidrule{1-3}
LLaMA-2 (7B) & 46.5 & 23.0 \\
\bottomrule
\end{tabular}
\caption{The performance of base LLaMA-2 (7B) and instruction tuned results using different data mixture.}
\label{tab:sft_proportion}
\end{table}
```

### 跨行跨列表

使用`\multicolumn{number}{alignment pattern}{content}`进行跨列，`\multirow`进行跨行。使用时，只需要将其视为一个命令整体，其他仍遵循表的基本排布方式。（&分列，\\分行）

```latex
\begin{table*}[!t]
\centering
\small
\resizebox{1\textwidth}{!}{
\begin{tabular}{cc|ccccccccc}
\toprule
\multicolumn{2}{c}{\textbf{LLaMA-2}} & \textbf{MMLU} & \textbf{BBH} & \textbf{HumanEval} & \textbf{NQs} & \textbf{HellaSwag} & \textbf{ARC-C} & \textbf{WinoGrande} & \textbf{BoolQ} & \textbf{GSM8K} \\
\midrule
\multirow{2}{*}{\textbf{7B}} & \textbf{Paper} & 45.3 & 32.6 & 12.8 & 25.7 & 77.2 & 45.9 & 69.2 & 77.4 & 14.6 \\
& \textbf{LLMBox} & 46.5 & 33.2 & 13.6 & 25.5 & 75.6 & 49.6 & 69.6 & 78.5 & 14.6 \\
\midrule
\multirow{2}{*}{\textbf{70B}} & \textbf{Paper} & 68.9 & 51.2 & 29.9 & 39.5 & 85.3 & 57.4 & 80.2 & 85.0 & 56.8 \\
& \textbf{LLMBox} & 69.5 & 54.8 & 29.2 & 40.3 & 83.3 & 57.8 & 80.7 & 85.6 & 56.6 \\
\bottomrule
\end{tabular}}
\caption{The results of different tasks on LLaMA-2 (7B) and (70B).}
\label{tab:dataset}
\end{table*}
```

