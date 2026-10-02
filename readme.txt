# Loop Engineering Template

SE開発を段階的にLoop Engineering化するための共通テンプレート。

## 基本原則

- 変更前に影響範囲を調査する
- 既存機能を不用意に変更しない
- 仕様を推測して変更しない
- TestにはEvidenceを残す
- PASS/FAILは可能な限り機械判定可能にする
- 設備安全に関わる処理はHuman Gateとする

## Files

- AGENTS.md
  - Codexが守る共通開発ルール
- templates/GOALS.template.md
  - 案件の目的・範囲・Done条件
- templates/TESTING.template.md
  - 案件のTest方法