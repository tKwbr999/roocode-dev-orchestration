# roocode-dev-orchestration
roo code(roo cline)で擬似マルチエージェントを実現する方法を模索

## 将来のアップデート待ち
- 平行処理の実現

## 処理フロー

### 1. タスク受付フェーズ
1. ユーザーがタスクを指示する
2. AI-P（進捗管理エージェント）がタスクを受け取る
3. AI-Pはタスクの基本情報を記録し、処理プロセスを初期化する

### 2. 分析・計画フェーズ
1. AI-PがAI-A（分析・技術選定エージェント）にタスクの分析を依頼する
2. AI-Aがタスク指示内容を詳細に分析する
   * 機能要件の抽出
   * 非機能要件の抽出
   * 制約条件の特定
3. AI-Aが最適な技術スタックを選定する
   * 言語・フレームワークの選定
   * ライブラリの選定
   * アーキテクチャパターンの決定
4. AI-Aが分析・選定結果をAI-Pに返却する

### 3. イシュー作成フェーズ
1. AI-Pが分析結果を受け取る
2. AI-Pが**親GitHubイシュー**を作成する
   * タイトルと説明の設定
   * 機能要件のリスト化
   * 技術スタック情報の記載
   * ラベルの設定（初期状態: 「準備中」）

### 4. 環境準備フェーズ
1. AI-PがAI-B（ディレクトリ作成エージェント）にディレクトリ作成を依頼する
2. AI-Bが親イシュー内容を読み取る
3. AI-Bが適切なプロジェクト構造を設計・作成する
4. AI-Bが親GitHubイシューに完了通知をコメントする
   * 作成したディレクトリ構造の詳細
   * イシューラベルを「環境準備完了」に更新

### 5. 実装フェーズ（並列処理）
1. AI-Pが親GitHubイシューを監視し、「環境準備完了」ラベルを検知する
2. AI-PがAI-T（テスト実装）とAI-D（開発実装）を同時に起動する

#### 5-A. テスト実装（AI-T）
1. AI-Tが親イシュー内容とプロジェクト構造を確認する
2. AI-Tがテストコードを実装する
3. AI-Tが親GitHubイシューに完了通知をコメントする
   * イシューへのラベル追加（「テスト実装完了」）

#### 5-B. 機能実装（AI-D）
1. AI-Dが親イシュー内容とプロジェクト構造を確認する
2. AI-Dが機能コードを実装する
3. AI-Dが親GitHubイシューに完了通知をコメントする
   * イシューへのラベル追加（「機能実装完了」）

### 6. 品質保証・レビューフェーズ
1. AI-Pが親GitHubイシューを監視し、「テスト実装完了」と「機能実装完了」の両方のラベルを検知する
2. AI-Pが改善要求回数カウンターを0に初期化する
3. AI-PがAI-Q（品質チェック兼コードレビュー）を呼び出す
4. AI-Qがテストと実装内容を検証する
   * テストの網羅性確認
   * 実装の品質確認
   * コードレビュー

#### 6-A. 改善が必要な場合
1. AI-QがGitHubイシューに改善要求をコメントする
   * イシューラベルを「改善要求」に更新
2. AI-Pが親GitHubイシューの「改善要求」ラベルを検知する
3. AI-Pが改善要求回数カウンターをインクリメントする
4. AI-Pが改善要求回数が3回未満かチェックする
   * 3回以上の場合は6-Bの「品質基準を満たした」ルートに強制的に進む
5. AI-Pが親イシューに紐づく**子イシュー**を作成する
   * 改善要求内容の詳細を記載
   * 担当エージェント（AI-TまたはAI-D）を指定
6. 担当エージェント（AI-TまたはAI-D）が子イシューを確認する
7. 担当エージェントが改善を実施する
8. 担当エージェントが子イシューをクローズし、親イシューにコメントする
9. AI-Pが親イシューの更新を検知し、再度AI-Qを呼び出す（ステップ6-4から繰り返し）

#### 6-B. 品質基準を満たした場合
1. AI-QがGitHubイシューに品質承認をコメントする
   * イシューラベルを「品質チェック・レビュー完了」に更新
2. AI-QがPull Requestを作成する

### 7. 完了フェーズ
1. AI-Pが親GitHubイシューの「品質チェック・レビュー完了」ラベルを検知する
2. AI-PがPull Requestをマージする
3. AI-Pが親イシューをクローズし、タスク完了とする
