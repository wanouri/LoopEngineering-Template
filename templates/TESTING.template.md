# TESTING.md

## 1. Purpose

このドキュメントは、本案件におけるBuild・Test・Evidence取得・PASS / FAIL判定の基本方法を定義する。

個別テストケースの詳細条件は、

```text
test-scenarios/
```

配下のテストケースを参照する。

案件の目的・完了条件については、

```text
docs/GOALS.md
```

を参照する。

---

# 2. Test Policy

テストは以下を基本方針とする。

1. 可能な限り再現可能な条件で実施する
2. PASS / FAIL条件を事前に定義する
3. 「正常に動作した」だけでPASSとしない
4. ログ・DB・状態値などEvidenceを取得する
5. FAIL時はプログラム修正前に試験条件を確認する
6. 既存改造では必要な回帰試験を行う
7. 設備関連では可能な限りSimulatorを使用する
8. 実機・本番環境に関する操作はHuman Gateとする

---

# 3. Test Environment

## Application

```text
Application:
Version:
Build:
Framework:
Configuration:
```

例:

```text
Application: SICS
Version:
Build: Debug
Framework: .NET Framework 4.5.2
Configuration: Test
```

---

## OS

```text
OS:
Machine:
```

---

## Database

```text
DBMS:
Database:
Server:
Schema:
```

必要に応じて記載する。

本番接続情報やPasswordなどの機密情報は記載しない。

---

## Simulator

使用するSimulatorを記載する。

```text
- Upper Simulator:
- PLC Simulator:
- Crane Simulator:
- Device Simulator:
- Other:
```

使用しない場合は `N/A`。

---

## External Systems

```text
- PLC:
- Host:
- Device:
- File System:
- Network:
```

---

# 4. Startup Procedure

試験環境の基本的な起動順を記載する。

```text
1.
2.
3.
4.
```

例:

```text
1. Database起動
2. PLC Simulator起動
3. 上位Simulator起動
4. 対象Application起動
5. 接続状態を確認
```

---

# 5. Initial State

試験開始前に必要な共通初期状態。

```text
- DB:
- Application:
- PLC:
- Simulator:
- File:
- Other:
```

例:

```text
- 対象搬送データが存在しない
- PLC SimulatorがONLINE状態
- 異常信号がOFF
- 前回試験のTracking Dataが残っていない
```

---

# 6. Build Procedure

## Build Command / Procedure

```text
Build方法:
```

例:

```text
Visual StudioでSolutionをBuildする。
```

または、

```text
dotnet build
```

---

## Build PASS

以下を満たすこと。

```text
- Build成功
- Error = 0
```

必要に応じてWarningの扱いを定義する。

---

## Build FAIL

以下の場合はFAILとする。

```text
- Compile Error
- Link Error
- 必要ファイル生成失敗
- 今回の変更に起因するBuild Error
```

---

# 7. Test Case Format

個別テストケースは原則として以下の形式とする。

```text
Test ID
Purpose
Preconditions
Input
Actions
Intermediate Assertions
Final Assertions
Evidence
PASS Conditions
FAIL Conditions
Human Check
```

保存場所:

```text
test-scenarios/
```

---

# 8. Test Execution Flow

個別試験は基本的に以下の順序で実施する。

```text
Prepare
   ↓
Set Preconditions
   ↓
Set Input
   ↓
Execute
   ↓
Observe
   ↓
Assert
   ↓
Collect Evidence
   ↓
PASS / FAIL
   ↓
Cleanup
```

---

# 9. Assertions

可能な限り機械判定可能な条件を使用する。

## Examples

### Application

```text
ReturnValue == expected
ErrorCode == 0
Exception == none
```

### Database

```text
COUNT(*) == expected
STATUS == expected
対象Recordが存在する
対象Recordが削除されている
```

### PLC

```text
DM == expected
Signal == ON
Signal == OFF
SEQ == expected_seq
```

### Communication

```text
ResponseCode == NORMAL
Command == expected
Timeout == false
```

### File

```text
File Exists == true
File Size > 0
Record Count == expected
```

### Log

```text
Expected Log Exists == true
ERROR Count == 0
Unexpected Exception == none
```

---

# 10. Timeout

状態変化待ちを無制限に行わない。

必要に応じてTimeoutを設定する。

```text
Default Timeout:
```

例:

```text
Default Timeout: 10 sec
```

個別ケースで異なる場合はテストケース側に記載する。

Timeout発生時は原則FAILとする。

ただし、仕様上Timeoutが期待結果となる異常系試験を除く。

---

# 11. Evidence

PASS / FAIL判定に使用したEvidenceを可能な限り保存する。

## Evidence Types

```text
- Application Log
- Communication Log
- DB SELECT Result
- PLC DM
- PLC Signal
- STATUS
- SEQ
- Output File
- Screenshot
- Test Runner Result
- git diff
```

---

## Evidence Location

基本保存場所:

```text
test-results/
```

推奨構成:

```text
test-results/
└─ <Test-ID>/
   ├─ result.md
   ├─ summary.txt
   └─ raw/
```

`raw/` には必要に応じて生ログ等を保存する。

---

# 12. PASS / FAIL

## PASS

以下をすべて満たす場合にPASSとする。

```text
- 必須Assertion成立
- Timeoutなし
- 想定外Errorなし
- 必要なEvidence取得済み
```

---

## FAIL

以下のいずれかに該当する場合はFAILとする。

```text
- Assertion不成立
- Timeout
- 想定外Exception
- 想定外Error Log
- DB状態不一致
- PLC値不一致
- 通信結果不一致
```

---

## Human Check

機械判定できない項目は、

```text
Pending Human Check
```

として機械PASSと区別する。

例:

```text
- 画面表示確認
- 操作性確認
- 設備安全確認
- 実機動作
```

---

# 13. Failure Analysis

FAIL発生時は、コード変更前に以下を確認する。

```text
1. Test Case
2. Preconditions
3. Input
4. Simulator
5. Configuration
6. Database
7. Environment
8. Program
```

原因を可能な範囲で分類する。

```text
Program
Test
Simulator
Configuration
Database
Environment
Requirement
Unknown
```

原因が確定していない状態で大きなコード変更を行わない。

---

# 14. Retest

不具合修正後は、最低限以下を実施する。

```text
1. FAILしたTest Caseの再実行
2. PASS確認
3. 必要な関連Testの実行
4. Evidence再取得
```

既存改造の場合は、必要に応じてRegression Testを実施する。

---

# 15. Regression Test

既存システムの改造では、変更箇所以外への影響を確認する。

Regression Scopeは、

```text
docs/GOALS.md
```

を参照する。

テスト対象例:

```text
- 既存正常系
- 共通処理
- 同一DBを使用する処理
- 同一PLC信号を使用する処理
- 同一通信処理
- 同一画面
```

---

# 16. Cleanup

各テスト終了後に、次の試験へ影響する状態を残さない。

必要に応じて、

```text
- Test Data削除
- DB状態復旧
- PLC Signal OFF
- DM初期化
- Simulator初期化
- 出力ファイル整理
```

を行う。

自動Cleanupが危険な場合はHuman Gateとする。

---

# 17. Test Automation Levels

本案件では必要に応じて以下の段階で自動化する。

```text
Level 0
Manual Execution
Manual Judgment

Level 1
Manual Execution
Automatic Judgment

Level 2
Automatic Simulator Operation
Automatic Judgment

Level 3
Automatic Evidence Collection
Multiple Test Execution

Level 4
Automatic Failure Analysis Support

Level 5
Automatic Retest Loop
```

必ずしも最高Levelを目指す必要はない。

費用対効果の高い範囲まで自動化する。

---

# 18. Test Result Format

個別テスト結果は最低限以下を残す。

```text
Test ID:
Executed:
Result:

Expected:
Actual:

Evidence:

Failure Category:

Notes:

Human Check:
```

---

# 19. Current Automation Scope

今回、自動化する範囲。

```text
Automatic:
- 

Manual:
- 

Human Gate:
- 
```

案件開始時点で完全に決定する必要はない。

試験を実施しながら更新してよい。

---

# 20. Improvement Notes

試験中に、

- 手作業が多い
- 同じ確認を繰り返している
- 判定が曖昧
- Evidence取得が面倒
- Simulator操作が定型化している

などを発見した場合は記録する。

```text
Automation Candidate:
Reason:
Expected Benefit:
Priority:
```

次のLoop Engineering改善候補として使用する。