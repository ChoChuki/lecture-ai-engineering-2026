# 第3回 解答

## 演習

- 第1回とエージェントの回答の違い：
- --traceで見えたツール呼び出し：search_glossary({'query': 'LoRA'})とsearch_glossary({'query': 'QLoRA'})（質問「LoRAとQLoRAの違いは？」）
- 1つの質問でのLLM呼び出し回数：2
- 追加したツールと、docstring に書いた説明：

## 宿題

- 取り組んだレベル：発展(a)
- やったこと：
  - 演習で使った学習支援エージェントPydantic AIと同じINSTRUCTIONSと同じツールを使い，エージェントを組み立てる部分だけをADKで書き直した．`adk_agent/glossary_agent/`に`agent.py`と`__init__.py`を作った
  - `adk run`で動かした．対話モードではなく，質問を引数で渡す形で実行した
  - 演習と同じ2つの質問（「3レベル」と「LoRAとQLoRAの違い」）を，Pydantic AI版と ADK版にそれぞれ1回ずつ投げて，書き方と回答を比べた
- 結果：
  - 書き方の違い：Pydantic AIではAgent(instructions=INSTRUCTIONS)でエージェントを作り，agent.tool_plain(...)でツールを1つずつ登録する．モデルは実行時に渡す．ADKではAgent(model=..., name=..., instruction=INSTRUCTIONS, tools=[...])と，モデル・名前・指示・ツールを1か所で渡す．ツール関数は書き換えずにそのまま渡せた．また，ADKはroot_agentという名前で置くとadk runが見つけて動かす
  - 回答の違い（3レベル）：どちらも用語集のとおりに正しく答えた．言い回しは少し違い，ADK版は「どれか1つ」を太字にしていた．末尾の回の表示は，Pydantic AI版が「第1回」，ADK版が「第1回（関係する回：第1回）」だった
  - 回答の違い（LoRAとQLoRAの違い）：どちらもLoRAとQLoRAの定義を説明できた．Pydantic AI版は--traceでsearch_glossaryをLoRAとQLoRAの2回呼んだことが分かり（requests=2，tool_calls=2），末尾に「関係する回：第2回」を示した．ADK版は冒頭で違いを1文にまとめてから説明し，末尾にはPydantic AI版と同じ「関係する回：第2回」を示した
  - 見えた情報の違い：Pydantic AIの--traceではツール呼び出しと呼び出し回数が見えた．ADKの通常の出力は最終回答だけで，ツール呼び出しは確認できなかった
- 気づき・疑問：
  - 指示とツール関数をそのまま使えたので，エージェントに必要な材料は，フレームワークが違っても同じだと確認できた
  - 同じモデル・同じ指示・同じツールでも，回答の書き方が少し違った．ただし各質問を1回ずつしか試していないので，フレームワークの違いなのか，LLMの回答の揺れなのかは区別できていない
  - 疑問：ADKでツール呼び出しやLLMの呼び出し回数を確認するにはどうすればよいか（今回は未確認）．また，ADKが内部で指示に何か足しているのかも気になった
