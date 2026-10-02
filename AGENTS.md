# AGENTS.md

## 1. Purpose

このリポジトリでは、以下の開発を対象とする。

- 既存システムの改造
- 機能追加
- 不具合修正
- 新規システム開発
- シミュレータ開発
- テストツール開発
- テスト自動化
- 保守・改善

基本方針は、

**要求を明確にし、必要な範囲を調査し、小さく実装し、Build・Test・Evidenceによって確認しながら段階的に完成させること**

である。

AI/Codexは原則として、

1. Goal確認
2. 現状・関連情報調査
3. 完了条件確認
4. 設計
5. 実装
6. Build
7. Test
8. Evidence取得
9. PASS / FAIL判定
10. 問題分析
11. 必要な修正
12. 再試験

のループで作業する。

---

# 2. Determine Development Type

作業開始時に、今回の開発を以下のいずれかに分類する。

## Existing System Modification

既存システムに対する、

- 改造
- 機能追加
- 不具合修正
- 仕様変更
- 保守

など。

この場合は既存動作への影響を特に重視する。

---

## New Development

新しく、

- アプリケーション
- サービス
- ツール
- シミュレータ
- テストツール
- サブシステム

などを作成する場合。

この場合は既存コードへの追従よりも、

- 要求との整合
- 単純な構成
- 保守性
- テスト容易性
- 将来の変更容易性

を重視する。

---

## Hybrid

既存システムに新規ツールや新規サブシステムを追加する場合。

例:

- 既存SICSに対する新規Simulator
- 既存システム向けログ解析ツール
- 既存設備向けテストランナー
- 既存アプリと連携する新規サービス

この場合、

**新規部分はNew Developmentとして設計し、接続部分はExisting System Modificationとして慎重に扱う。**

---

# 3. Common Principles

開発種別に関係なく以下を守る。

## 3.1 Understand before changing

最初に目的と完了条件を理解する。

コードを書き始めることを作業開始とは考えない。

---

## 3.2 Small iterations

一度に大量の機能を実装しない。

可能な限り、

```text
Small Change
    ↓
Build
    ↓
Test
    ↓
Evidence
    ↓
Review
```

を繰り返す。

---

## 3.3 Do not guess specifications

不明な仕様を推測だけで確定しない。

以下から確認する。

- GOALS.md
- 現行ソース
- 設計資料
- DB
- SQL
- 設定ファイル
- ログ
- 類似処理
- テスト項目
- Simulator
- 関連ドキュメント

事実と推測を区別する。

必要に応じて、

- Confirmed
- Assumption
- Unknown

として整理する。

---

## 3.4 Avoid unnecessary complexity

要求を満たすために必要以上に複雑な仕組みを導入しない。

特に新規開発では、

「将来使うかもしれない」

だけを理由に過剰な抽象化や汎用化を行わない。

---

# 4. Existing System Modification Rules

既存システムの改造では、既存動作への影響を最小限にする。

## 4.1 Preserve existing behavior

改造対象以外の既存機能を可能な限り変更しない。

既存処理を変更する場合は、

- なぜ必要か
- どこへ影響するか
- 何を回帰確認するか

を明確にする。

---

## 4.2 Make the smallest practical change

明示的な理由がない限り、以下を行わない。

- 大規模リファクタリング
- 全面的な構造変更
- DB構造変更
- 通信仕様変更
- PLCインターフェース変更
- 既存ログ形式変更
- Framework更新
- ライブラリ大量更新

必要な場合は理由と影響範囲を整理する。

---

## 4.3 Preserve existing project style

既存の、

- 命名規則
- コーディングスタイル
- DBアクセス方式
- エラー処理
- 通信方式
- ログ方式

を基本的に優先する。

---

## 4.4 Avoid unrelated changes

今回のGoalと関係のない変更を同時に行わない。

問題を発見した場合は、

`Additional Finding`

として報告し、必要なら別タスクとする。

---

# 5. New Development Rules

新規開発では、既存コードとの互換性よりも要求を満たす適切な設計を優先する。

ただし、不必要に大規模または複雑な構成にはしない。

---

## 5.1 Start from requirements

実装前に最低限以下を明確にする。

- Purpose
- Users
- Input
- Output
- Main Flow
- Error Cases
- External Interfaces
- Constraints
- Done Conditions

不明なものは明示する。

---

## 5.2 Design for testability

新規開発では最初からテストしやすい構造を優先する。

可能であれば、

- 入力
- 処理
- 外部I/O
- 状態管理
- 判定処理

を分離する。

特に設備Simulatorやテストツールでは、

```text
Input
 ↓
Action
 ↓
Wait
 ↓
Observe
 ↓
Assert
```

を分離可能な設計とする。

---

## 5.3 Prefer simple architecture

新規開発では最初から将来すべての要求を予測しない。

最初に必要な範囲を満たす最小構成を作る。

拡張が必要になった時点で構造を見直す。

---

## 5.4 Technology selection

新しいFramework、Library、Databaseなどを採用する場合は、

- 採用理由
- 現在の環境との整合
- 保守性
- 長期利用可能性
- 導入コスト

を確認する。

単に新しいという理由だけで採用しない。

一方、新規開発では既存システムと無関係な古い技術へ合わせる必要もない。

目的に適した技術を選択する。

---

## 5.5 Establish project conventions

新規開発では必要に応じて、

- ディレクトリ構成
- 命名規則
- ログ方式
- 設定方式
- エラー処理
- Test方式

を初期段階で定義する。

ただし、最初から巨大な規約を作らない。

実際の開発に合わせて更新する。

---

# 6. Before Editing or Implementing

`docs/GOALS.md` が存在する場合は最初に確認する。

以下を把握する。

- Objective
- Scope
- Out of Scope
- Done Conditions
- Human Gates
- Regression Scope

既存改造の場合はさらに、

- 呼び出し元
- 呼び出し先
- 共通関数
- DB更新
- 状態遷移
- 通信
- PLC I/O
- タイマー
- エラー処理
- ログ
- 類似処理

を調査する。

新規開発の場合は、

- 入出力
- 外部Interface
- 主な状態
- エラーケース
- Test方法

を整理する。

---

# 7. Impact Analysis

実装前または実装初期に、必要に応じて以下を整理する。

## Existing Modification

- 変更ファイル
- 変更関数
- 関連機能
- DB影響
- PLC影響
- 通信影響
- 設定影響
- 回帰対象

## New Development

- 新規コンポーネント
- 外部依存
- Interface
- DB
- 設定
- 通信
- Deployment
- Test対象

---

# 8. Configuration

IPアドレス、Port、接続先、PLC番号、設備番号、DB接続先などをコードへ不用意に固定しない。

既存改造の場合は既存方式を尊重する。

新規開発の場合は、変更される可能性が高い値は設定として分離することを検討する。

BuildやDebugによる設定ファイル上書きにも注意する。

---

# 9. Database Rules

DBに関する変更は慎重に扱う。

既存システムでは、明示的な理由なしに以下を変更しない。

- Table
- Column
- Index
- Stored Procedure
- Trigger

新規開発では必要に応じてDB設計を行ってよい。

ただし、

- データの意味
- Key
- NULL可否
- 更新主体
- 削除条件

を明確にする。

本番DBへの変更はHuman Gateとする。

---

# 10. PLC / Equipment Rules

PLC・設備連携では安全性を優先する。

## Simulation first

可能な限りSimulatorを使用する。

以下はSIMによる再現を優先する。

- DM
- SEQ
- 到着信号
- 発進信号
- 荷積可
- 荷降可
- 在荷
- 異常信号
- 完了信号

---

## Real equipment

明示的な許可なしに以下を自動実行しない。

- 実PLC書込み
- 実設備への搬送指示
- 実クレーン操作
- インターロック解除
- 強制ON/OFF
- 本番復旧操作

これらはHuman Gateとする。

---

# 11. Build Rules

変更後または機能追加後は可能な限りBuildする。

Build失敗時は、

1. 最初のError確認
2. 今回変更との関係確認
3. 原因特定
4. 必要な修正
5. Rebuild

の順に対応する。

エラーを消すためだけの無関係な大量変更を行わない。

---

# 12. Testing Rules

テスト方法は `docs/TESTING.md` を参照する。

テストケースは原則として、

`test-scenarios/`

に配置する。

可能な限り以下を明確にする。

- Test ID
- Purpose
- Preconditions
- Input
- Actions
- Intermediate Assertions
- Final Assertions
- Evidence
- PASS Conditions
- FAIL Conditions

---

# 13. Machine-verifiable Assertions

可能な条件は機械判定可能にする。

避ける:

`正常に処理されること`

推奨:

```text
STATUS = 81
SEQ == expected_seq
D7350 == expected_seq
COUNT(*) = 0
ResponseCode = NORMAL
ERROR count = 0
Timeout = false
```

画面表示など自動判定が難しいものはHuman Checkとして分離する。

---

# 14. Evidence

テストでは可能な限りEvidenceを残す。

例:

- Log
- DB SELECT結果
- STATUS
- PLC DM
- SEQ
- 通信電文
- 出力ファイル
- Test result
- Screenshot
- git diff

PASSした場合、

**何を根拠にPASSとしたか追跡できること**

を重視する。

---

# 15. Failure Handling

FAIL時に即座にコードを修正しない。

最初に、

1. Test条件
2. Input
3. Simulator
4. Configuration
5. Database
6. Environment
7. Program

を確認する。

原因を可能な範囲で、

- Program
- Test
- Simulator
- Configuration
- Database
- Environment
- Requirement
- Unknown

として整理する。

---

# 16. Simulator and Test Tool Development

SimulatorやTest Toolは新規開発として扱う。

ただし目的は、

**本物を完全再現することではなく、必要な試験を安定して再現できること**

である。

設計では、

```text
Set Condition
     ↓
Execute Action
     ↓
Wait
     ↓
Observe
     ↓
Assert
     ↓
Record Evidence
```

を意識する。

将来的に自動実行可能な構造を優先する。

---

# 17. Automation Rules

一度に全てを自動化しない。

基本的な発展順序は、

```text
Manual Test
    ↓
Automatic Judgment
    ↓
Automatic Simulator Operation
    ↓
Automatic Evidence Collection
    ↓
Multiple Test Execution
    ↓
Failure Analysis
    ↓
Automatic Retest
```

とする。

自動化コストが効果を上回る場合は手動を残す。

---

# 18. Human Gates

以下は原則として人間確認を残す。

- 設備安全
- インターロック
- 実PLC操作
- 実設備操作
- 本番DB変更
- 客先仕様判断
- 運用変更
- 本番反映
- 復旧操作
- 重大なArchitecture変更

AIは判断材料を整理できるが、許可なく実行しない。

---

# 19. Definition of Done

コードを書いたことだけを完了とはしない。

原則として、

- Goalを満たす
- Build成功
- 必要なTest実施
- PASS条件成立
- Evidence取得
- 重大な未解決事項がない
- 必要なHuman Check完了

を確認する。

既存改造ではさらに、

- 必要な回帰試験完了
- 既存機能への重大な影響なし

を確認する。

個別案件では `docs/GOALS.md` のDone Conditionsを優先する。

---

# 20. Reporting

作業完了時は最低限以下を報告する。

## Summary

今回実施した内容。

## Changed

変更内容。

## Files

変更したファイル。

## Verification

BuildおよびTest結果。

## Evidence

PASS判断の根拠。

## Remaining Issues

未解決事項。

## Additional Findings

今回のGoal外で発見した事項。

## Human Check

人間確認が必要な事項。

---

# 21. When Unsure

判断に迷った場合は、まず調査する。

特に、

- Requirement不明
- 既存処理の意図不明
- PLC信号不明
- DBデータ意味不明
- 客先運用不明
- 安全影響不明

の場合は、推測のみで重要な変更を進めない。

一方、新規開発で細部の仕様が未確定の場合は、

- Assumptionを明示
- 変更しやすい構造にする
- 最小実装で検証する

ことを優先する。

---

# 22. Core Working Loop

基本ループは開発種別によらず同じとする。

```text
Understand
    ↓
Investigate
    ↓
Define Done
    ↓
Design
    ↓
Implement
    ↓
Build
    ↓
Test
    ↓
Collect Evidence
    ↓
Judge PASS / FAIL
    ↓
Analyze
    ↓
Improve
    ↓
Retest
```

既存改造では、

**安全に変えること**

を重視する。

新規開発では、

**小さく作り、早く検証し、必要に応じて設計を育てること**

を重視する。

共通の目的は、

**機械が得意な調査・確認・反復・証跡収集を活用し、人間が要求・設計・安全・仕様判断に集中できる開発環境を作ることである。**