# context-free grammar

tags
: [[programming language implementation]]

A context-free grammar (CFG) is a set of recursive rules that describe a [[programming language]].

Context-free grammars are a set of names paired with their parsing rules.

For example:

\begin{example}
\mathit{expr} \rightarrow \mathit{term}\\; \mathtt{+}\\; \mathit{expr}

\mathit{expr} \rightarrow \mathit{term}

\mathit{term} \rightarrow \mathit{term}\\; \mathtt{\*}\\; \mathit{factor}

\mathit{term} \rightarrow \mathit{factor}

\mathit{factor} \rightarrow \mathtt{(}\\;\mathit{expr}\\;\mathtt{)}

\mathit{factor} \rightarrow \mathit{const}

\mathit{const} \rightarrow \mathit{integer}
\end{example}

Therefore we know `3 * 7` is a valid expression.

