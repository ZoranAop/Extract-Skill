# Extract Skill

本、長動画、ポッドキャストの方法論を呼び出し可能な AI スキルに蒸留します。

## コア能力

extract-skill は要約ツールではなく、**エージェント実行向けの方法論蒸留パイプライン**です。本、長動画、ポッドキャストの方法論を、エージェントが正確に呼び出せるスキルパックに変えます。

| 能力 | 何をするか | なぜ重要か |
|---|---|---|
| **三重検証** | クロスドメイン + 予測力 + 独自性の3関門、合格率25-50%のみ | 金言と常識を濾過し、真に再利用可能な方法論のみ残す |
| **RIA++ 構造化** | R/I/A1/A2/E/B の6次元構築、A2=トリガーシーン、E=実行ステップ、B=境界 | 人間の読書向けではなくエージェント呼び出し向け、スキルが正確に発火 |
| **ストレステスト** | 囮プロンプトとスキル間混同テストによる独立ブラインドテスト | リリース前にトリガー精度の問題を発見、運任せにしない |
| **ツェッテルカステン連携** | depends-on / contrasts-with / composes-with の3関係 + 参照グラフ | スキル同士が組み合わせ可能、孤立したファイルではない |
| **darwin 互換** | 各スキルに `test-prompts.json` を同梱 | 产出は darwin-skill に直接接続して自動進化可能 |

## なぜこれを作ったのか

多くの本を読み、多くの動画を保存し、多くのポッドキャストを聴いていても、活用できない——知識は「読んだ」レベルに留まり、実際の意思決定で活性化されない。要約、メモ、字幕整理は**圧縮**であって**再利用**ではない：「いつ何を使うべきか」が分からないまま。また、高価値な内容の中で本当にツールになる価値のあるものはごく一部で、全部入りにすると価値が薄まる。

extract-skill の目標は一つ：**長文コンテンツの方法論を呼び出し可能な AI スキルパックに蒸留すること**。本だけでなく、字幕/トランスクリプトのある動画、ポッドキャスト、インタビュー、講演、コース、長文、資料集にも対応。内容に抽出可能、検証可能、移転可能な方法論があれば、extract-skill で独立して呼び出し可能、組み合わせ可能、ストレステスト可能なスキルに変えられます。

動画コンテンツを蒸留する場合は、[video-downloader](https://github.com/kangarooking/kangarooking-skills/tree/main/video-downloader) skill との併用を推奨：先に動画をダウンロードして字幕/音声トランスクリプトを抽出し、そのテキストを extract-skill に渡して方法論抽出、スキル化、ストレステストを行います。

## 解決する問題

| ペインポイント | extract-skill での解決 |
|---|---|
| 多くを読む/観る/聴くが活用できない——知識が「見た」レベルに留まる | トリガー条件付きのスキルを产出、エージェントが実シーンで呼び出し |
| 要約/メモは圧縮であり構造化再利用ではない——「いつ何を」が分からない | A2 フィールドがトリガーシーン + 言語シグナルを明示、スキルがオンデマンドで発火 |
| ツール化する価値のある内容はごく一部 | 三重検証でフィルタリング、合格率25-50%、金言と常識を除去 |
| 既存の方法論は人間の読者向け、エージェント実行者向けではない | RIA++ 構造：E=実行ステップ、B=境界、消費向けではなく実行向け |

## どう動くか

extract-skill は **RIA-TV++** パイプラインを使用して、本を生のテキストから構造化されたスキルのセットに変換します。7段階のプロセスです：

1. **全書理解（Adler分析）** — モーティマー・アドラーの分析方法で、全書を構造・解釈・批判・応用の4ステップで分解し、`BOOK_OVERVIEW.md` を生成
2. **並行抽出** — 5つの専門エクストラクター（フレームワーク、原則、事例、反例、用語）が同時に実行され、原文から候補ユニットを抽出
3. **三重検証** — 各候補は3つのチェックを通過する必要があります：書中に少なくとも2つの独立した裏付けがあるか（クロスドメイン）、新しい質問に答えられるか（予測力）、常識ではないか（独自性）。合格率は通常25〜50%
4. **RIA++ 構築** — 検証済みの内容を6つの次元に構造化：R（原文引用）/ I（自分の言葉での再構築）/ A1（書中の事例）/ A2（将来のトリガーシーン）/ E（実行可能ステップ）/ B（境界と盲点）
5. **ツェッテルカステン連携** — スキル間の依存、対比、構成関係を特定し、参照グラフ付きの `INDEX.md` を生成
6. **ストレステスト** — 各スキルに囮問題（スキル間混同テストを含む）を含むテストプロンプトを設計。不合格は全面的に再構築
7. **デリバリー** — 読者向けの `DIGEST.md`（本を読まずにエッセンスを読める長文ダイジェスト）を生成し、テストに合格したスキルを Claude Code / Cursor の skills ディレクトリにインストールして、実際に呼び出せるようにする

RIA-TV++ の名前の由来：
- **RIA**：趙周のブックマーク法（Reading / Interpretation / Appropriation）
- **TV**：Triple Verification（三重検証）
- **++**：エージェント実行向けの拡張——E（Execution）+ B（Boundary）

## クイックスタート

### 前提条件

- Claude Code、Cursor、またはスキル読み込みをサポートするその他のエージェントホスト
- 蒸留するソーステキスト（PDF / EPUB / TXT / 字幕ファイル / トランスクリプト）。動画/ポッドキャストは先にトランスクリプトを取得

### 使い方

エージェントに以下のいずれかを言うと、extract-skill が自動的に起動します：

```
帮我拆《穷查理宝典》
distill this book into skills: <path>
turn this video/podcast/course into skills
```

パイプラインは段階0から開始し、各段階完了後に進捗を報告して確認を求めます。ブレークポイントからの再開をサポート。完全な実行仕様は [SKILL.md](./SKILL.md)、各段階の設計説明は [methodology/](./methodology/) を参照してください。

## 効果例

### 例1：本からスキルパックへ

**ユーザーの要望**

「本のコア方法論を再利用可能な AI スキルにしたい。読書要約ではなく。」

**extract-skill の判断**

- 元資料に再利用可能な方法論ユニットがあるか確認
- スタンドアロンのスキルに値するものと背景情報を区別
- 単一の要約ではなく、構造化されたスキルリポジトリを出力

**出力例**

> 結果は1つの要約ファイルではなく、`BOOK_OVERVIEW.md`、`INDEX.md`、読者向けの `DIGEST.md`、`GLOSSARY.md`、複数の `*/SKILL.md`、トリガー検証用の `test-prompts.json` を含む multi-skill リポジトリになります。

### 例2：圧縮ではなく、構造化再利用

**ユーザーの要望**

「長い説明文が欲しいのではなく、エージェントが再利用できるスキルパックが欲しい。」

**extract-skill の判断**

- 目標は構造化再利用であり、物語の圧縮ではない
- トリガー可能、組み合わせ可能、テスト可能なスキルユニットを優先
- スタンドアロンのスキルに値しない素材は落とす

**出力例**

> システムはトリガー条件、境界、実行パターン、関連スキルリンクを持つ複数のスキルモジュールを生成します——全体を1つの汎用的なノートに平坦化するのではなく。

## 生成済みスキルパック

| リポジトリ | 元資料 | スキル数 |
|------------|--------|----------|
| [buffett-letters-skill](https://github.com/kangarooking/buffett-letters-skill) | バフェットの株主への手紙（1957-2023） | 20 |
| [cognitive-dividend-skill](https://github.com/kangarooking/cognitive-dividend-skill) | 認知の红利 | 15 |
| [duan-yongping-skill](https://github.com/kangarooking/duan-yongping-skill) | 段永平の投資Q&A（ビジネス+投資ロジック） | 15 |
| [viral-copywriting-skill](https://github.com/kangarooking/viral-copywriting-skill) | 『爆款文案』 | 14 |
| [copywriters-handbook-skill](https://github.com/kangarooking/copywriters-handbook-skill) | 『The Copywriter's Handbook』 | 12 |
| [contagious-skill](https://github.com/kangarooking/contagious-skill) | 『Contagious』 | 15 |
| [influence-skill](https://github.com/kangarooking/influence-skill) | 『Influence』 | 12 |
| [1000-true-fans-skill](https://github.com/kangarooking/1000-true-fans-skill) | 『1000 True Fans』 | 13 |
| [system-prompt-skills](https://github.com/kangarooking/system-prompt-skills) | 165個のAI製品システムプロンプト | 15 |
| [X-growth-skills](https://github.com/kangarooking/X-growth-skills) | X（Twitter）のアカウント立ち上げ、コンテンツ成長、アルゴリズム、交流、収益化の実践資料 | 15 |
| [poor-charlies-almanack-skill](https://github.com/kangarooking/poor-charlies-almanack-skill) | 貧しきチャーリーの格言 | 12 |
| [no-rules-rules-skill](https://github.com/kangarooking/no-rules-rules-skill) | No Rules Rules | 10 |
| 黄帝内経・素問（本プロジェクト内） | 『黄帝内経・素問』 | 10 |
| 黄帝内経・霊枢（本プロジェクト内） | 『黄帝内経・霊枢』 | 8 |
| [first-principles-skill](https://github.com/kangarooking/first-principles-skill) | 『第一性原理』 | 10 |
| [mao-selected-works-skill](https://github.com/kangarooking/mao-selected-works-skill) | 『毛沢東選集』第1-5巻 | 25 |
| [qbdx-hub/buffett-letters-skill](https://github.com/qbdx-hub/buffett-letters-skill) | バフェット株主への手紙（1957-2023） | 20 |
| [qbdx-hub/wo-yu-di-tan-skill](https://github.com/qbdx-hub/wo-yu-di-tan-skill) | 『我与地坛』 | 6 |
| [qbdx-hub/mingchao-those-things-skill](https://github.com/qbdx-hub/mingchao-those-things-skill) | 『明朝那些事儿』 | 7 |
| [qbdx-hub/sunzi-bingfa-skill](https://github.com/qbdx-hub/sunzi-bingfa-skill) | 『孫子兵法』 | 8 |
| [qbdx-hub/zhouyi-skill](https://github.com/qbdx-hub/zhouyi-skill) | 『周易』 | 8 |
| [qbdx-hub/high-math-vol1-ch1-skill](https://github.com/qbdx-hub/high-math-vol1-ch1-skill) | 高等数学上冊第1章 | 8 |

より多くの高価値な本の蒸留を計画中。

追加の外部ソース（著者本人の許可を得て掲載）：

- 元リポジトリ: [ace3000chao/book2startup](https://github.com/ace3000chao/book2startup)
- 書目: 『リーン・スタートアップ』『孫子兵法』『荘子』『易経』
- 元リポジトリ: [shenqistart/book2skill](https://github.com/shenqistart/book2skill)
- 書目: 『纏論』『茶経』

## リポジトリ構造

```text
extract-skill/
├── README.md              ← 今見ているファイル
├── README.en.md           ← 英語版
├── README.ja.md           ← 日本語版
├── LICENSE                ← MIT
├── SKILL.md               ← メタスキル定義（extract-skill の完全な実行仕様）
├── methodology/           ← RIA-TV++ の段階別方法論ドキュメント
├── extractors/            ← 5つの並行エクストラクターのプロンプト定義
└── templates/             ← SKILL / INDEX / BOOK_OVERVIEW / DIGEST / GLOSSARY / verified / PIPELINE_STATE / test-prompts テンプレート
```

## エコシステム

extract-skill はより大きなスキルエコシステムの一部です：

- [nuwa-skill](https://github.com/alchaincyf/nuwa-skill) — 人を蒸留する（思考スタイル、表現 DNA）
- **extract-skill**（このリポジトリ）— 本を蒸留する（方法論、フレームワーク、原則）
- [darwin-skill](https://github.com/alchaincyf/darwin-skill) — 任意のスキルを進化させる

これらは連携しています：nuwa は人を蒸留し、extract は本を蒸留し、darwin はそれらを進化させ続けます。

## More Skills

- [Buffett Letters Skill](https://github.com/kangarooking/buffett-letters-skill) — バフェットの60年以上の株主への手紙から抽出した20個の投資判断スキル
- [Poor Charlie's Almanack Skill](https://github.com/kangarooking/poor-charlies-almanack-skill) — チャーリー・マンガーのコア思考法から抽出した12個の意思決定・判断スキル
- [No Rules Rules Skill](https://github.com/kangarooking/no-rules-rules-skill) — ネットフリックスの自由と責任の文化から抽出した10個の組織設計スキル
- [Cognitive Dividend Skill](https://github.com/kangarooking/cognitive-dividend-skill) — 『認知の红利』から抽出した15個の認知ツールスキル
- [Duan Yongping Skill](https://github.com/kangarooking/duan-yongping-skill) — 段永平の投資Q&Aから抽出した15個のビジネス・投資スキル
- [Viral Copywriting Skill](https://github.com/kangarooking/viral-copywriting-skill) — 『爆款文案』から抽出した14個の販売コピーライティング・診断スキル
- [Copywriters Handbook Skill](https://github.com/kangarooking/copywriters-handbook-skill) — 『The Copywriter's Handbook』から抽出した12個の販売コピー・見出し・ベネフィット変換スキル
- [Contagious Skill](https://github.com/kangarooking/contagious-skill) — 『Contagious』から抽出した15個のSTEPPS伝播戦略・口コミ診断スキル
- [Influence Skill](https://github.com/kangarooking/influence-skill) — 『Influence』から抽出した12個の説得心理・コンプライアンス機制・防御判断スキル
- [1000 True Fans Skill](https://github.com/kangarooking/1000-true-fans-skill) — 『1000 True Fans』から抽出した13個の個人ブランド・真のファン育成・信頼ベース収益化スキル
- [System Prompt Skills](https://github.com/kangarooking/system-prompt-skills) — 165個のAI製品システムプロンプトから抽出した15個のシステムプロンプト設計スキル
- [X Growth Skills](https://github.com/kangarooking/X-growth-skills) — Xの立ち上げ、コンテンツ、アルゴリズム、交流、分析、収益化の15スキル
- Huangdi Neijing Suwen Skill（本プロジェクト内）— 『黄帝内経・素問』から抽出した10個の中医観察・調整スキル
- Huangdi Neijing Lingshu Skill（本プロジェクト内）— 『黄帝内経・霊枢』から抽出した8個の心身調整・弁証スキル
- [First Principles Skill](https://github.com/kangarooking/first-principles-skill) — 『第一性原理』から抽出した10個の公理化思考・破界イノベーション・組織刷新スキル
- [Mao Selected Works Skill](https://github.com/kangarooking/mao-selected-works-skill) — 『毛沢東選集』第1-5巻から抽出した25個の認知・戦略・組織・実行スキル
- [qbdx-hub Buffett Letters Skill](https://github.com/qbdx-hub/buffett-letters-skill) — バフェット株主への手紙から抽出した20個の投資・資本配分スキル
- [qbdx-hub Wo Yu Di Tan Skill](https://github.com/qbdx-hub/wo-yu-di-tan-skill) — 『我与地坛』から抽出した6個の制約・苦難・執筆・自己定位スキル
- [qbdx-hub Mingchao Those Things Skill](https://github.com/qbdx-hub/mingchao-those-things-skill) — 『明朝那些事儿』から抽出した7個の権力構造・制度失敗・歴史説明スキル
- [qbdx-hub Sunzi Bingfa Skill](https://github.com/qbdx-hub/sunzi-bingfa-skill) — 『孫子兵法』から抽出した8個の戦略判断・資源制御・行動選択スキル
- [qbdx-hub Zhouyi Skill](https://github.com/qbdx-hub/zhouyi-skill) — 『周易』から抽出した8個の状況診断・時機判断・進退境界スキル
- [qbdx-hub High Math Vol. 1 Chapter 1 Skill](https://github.com/qbdx-hub/high-math-vol1-ch1-skill) — 高等数学上冊第1章から抽出した8個の極限・無限小・連続性学習スキル

External Source（著者本人の許可を得て掲載）:

- [book2startup](https://github.com/ace3000chao/book2startup) — 『リーン・スタートアップ』『孫子兵法』『荘子』『易経』を蒸留した skills を含む
- [book2skill](https://github.com/shenqistart/book2skill) — 『纏論』『茶経』を蒸留した AI-Agent skills を含む

## License

MIT. See [LICENSE](./LICENSE).
