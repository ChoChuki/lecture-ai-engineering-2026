# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：
  - aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要', 'topics': ['general']})
  - aws_aws___search_documentation({'search_phrase': '"Versioning" "Amazon S3" what is', 'topics': ['general']})
- 気づき・疑問：
  - 設定ファイルに書き足しただけで，エージェントのコードは変えていないのに，モデルに見えるツールが1個→2個→7個と増えた
  - STEP 4では，1回目の検索（日本語を含む語句）の1位がS3のメタデータを調べるスキル（`querying-aws-s3`）で，バージョニングの説明ではなかった．モデルは英語の語句に変えて検索し直し，2回目の1位は「Amazon S3 FAQs」だった．日本語で検索したことが原因かどうかは確かめていない
  - 入力トークン数はSTEP 2・3・4で715→2,230→11,540と増えた．質問が違うので単純には比べられないが，ツールが増えて説明文が長くなったことや，検索結果が長いことが関係しているのか気になった（未確認）

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：発展
- やったこと：
  - 調べたソフトウェアはVS Code（演習で使っているCodespacesの中のエディタ）．MCPサーバーの設定方法を，公式ドキュメントで調べた
  - 確認日は2026年11月26日．参照した公式ドキュメントは次の3つ：
    - https://code.visualstudio.com/docs/agents/reference/mcp-configuration
    - https://code.visualstudio.com/docs/agent-customization/mcp-servers
    - https://code.visualstudio.com/docs/agents/run/manage-mcp-servers
  - 演習の`app/mcp_servers.json`（`glossary`・`fetch`・`aws`の3つを書いたもの）と，VS Codeの設定方法を，「設定を書く場所」「`command`にあたる書き方」「`url`にあたる書き方」「有効になる範囲」の4点で比べた
  - 上の3つのページを自分で開き，次の点を確認した：設定ファイルの2つの置き場所とトップレベルの`mcpServers`，stdioとhttpの書き方，旧形式の非推奨と削除予定，Copilot Globalと`.mcp.json`の役割，Dev Containerの`customizations.vscode.mcp`，ローカルのユーザーファイルがリモートに転送されないこと
- 結果：
  - 設定を書く場所：
    - 演習：`app/mcp_servers.json`（エージェントが起動時に読む）
    - VS Code：プロジェクト用はワークスペース直下の`.mcp.json`，個人用は`~/.copilot/mcp-config.json`
  - `command`にあたる書き方：
    - 演習：`"command"`と`"args"`を書く（`type`は書かない）
    - VS Code：`"type": "stdio"`に加えて，`"command"`と`"args"`を書く
  - `url`にあたる書き方：
    - 演習：`"url"`だけを書く
    - VS Code：`"type": "http"`に加えて，`"url"`を書く
  - 有効になる範囲：
    - 演習：このプロジェクトだけ（ファイルがプロジェクトの中にあるため）
    - VS Code：`.mcp.json`はそのワークスペースだけで，gitで共有できる．`~/.copilot/mcp-config.json`はすべてのワークスペースで共通
  - 補足：トップレベルのキーは，演習もVS Codeの現行の推奨形式も`mcpServers`で同じだった．VS Codeの旧形式（`.vscode/mcp.json`など）は`servers`で，この形式を使うLocal harnessは非推奨とされ，2027年2月に削除される予定と書かれていた
  - Codespacesで使う場合：ローカルのCopilotのユーザーファイルはリモートに転送されない．コンテナの作成時に設定を入れたいときは，`devcontainer.json`の`customizations.vscode.mcp`に書く方法がある（VS Code独自の形式）
- 気づき・疑問：
  - 同じMCPでも，設定の形式はソフトウェアごとに違う．VS Codeの中でも，現行の形式は`mcpServers`，旧形式は`servers`と分かれていた（講義資料の23ページにも，書式はホストによって異なるとある）
  - ドキュメントの更新が速い．参照したページの日付は前日の2026年11月25日で，旧形式はすでに非推奨になっていた．設定方法はよく変わると実感した
  - 同じCodespacesの中に，MCPクライアントの役割を果たすものが少なくとも2つある．演習のエージェントは`app/mcp_servers.json`を，VS CodeのCopilotは`.mcp.json`などを読むので，設定ファイルは別々だった
  - 疑問：演習のファイルは`type`を書いていないが，VS Codeのドキュメントの例はどれも`type`を書いている．省略できるかどうかは，確認した範囲では書かれていなかった