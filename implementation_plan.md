# Quarto Implementation Plan

実物のクアルトのルール（特に宣言漏れ指摘）を忠実に再現した、ローカル動作可能なウェブゲームを構築します。

## Proposed Changes

### [Component Name] Core Logistics
単一の `index.html` に HTML, CSS, JavaScript を集約し、外部依存なしで動作させます。

#### [NEW] [index.html](file:///c:/Users/big_b/.gemini/antigravity/brain/quarto-game/index.html)
- **Data Structure**:
  - `Piece`: `bits` (4ビットで属性を表現: 0000 to 1111)
  - `Board`: 16マスの配列。
  - `Phase`: `CHOOSING` (コマ選択), `PLACING` (コマ配置), `WAITING_DECLARATION` (宣言待機)
- **Win Condition**:
  - 4つのコマのビット論理積 (AND) またはビット否定の論理積が 0 でない属性があるか判定。
- **Declaration Rule**:
  - `WAITING_DECLARATION` フェーズを厳密に設け、自分が置いた後、または相手が置いた後に宣言ボタンを押せるようにします。
- **NPC AI**:
  - Lv 1: ランダム。
  - Lv 2: 自分が勝てる手、または相手のクアルトを防ぐ手を優先。
  - Lv 3: 1手先を読み、相手に勝ち目を与えないコマを渡す。

### UI Design
- **Aesthetics**: Glassmorphism を採用し、プレミアムな雰囲気を作ります。
- **Layout**: 
  - 左側に 4x4 盤面。
  - 右側に未使用コマ一覧。
  - 下部に状態表示と操作ボタン。
- **Piece Rendering**: SVG または CSS の擬似要素を使用して、高さ・穴・形・色を表現します。

## Verification Plan

### Manual Verification
1.  **通常勝利**: 自分がコマを置いた直後に「クアルト」を宣言して勝利することを確認。
2.  **宣言漏れ指摘**: 相手がリーチを完成させたが宣言せずにコマ選択フェーズに移ろうとした際（またはその待機中）に、自分が宣言して勝利することを確認。
3.  **誤宣言敗北**: クアルトが成立していないのに宣言して敗北することを確認。
4.  **NPC対戦**: 各レベルのNPCが適切に動作することを確認。
5.  **ローカル動作**: ファイルを直接開いて全ての機能（画像含む）が動作することを確認（SVGインライン化などで対応）。
