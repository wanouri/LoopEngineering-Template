# TEST CASE

## 1. Test Information

```text
Test ID:
Title:
Category:
Priority:
```

### Category

```text
Normal
Abnormal
Boundary
Regression
Recovery
Other
```

---

## 2. Purpose

このテストで何を確認するかを記載する。

```text
例:
登録処理実行時に、入力したデータがDBへ正しく登録されることを確認する。
```

---

## 3. Related Goal

`docs/GOALS.md` のどの目的・Done Conditionに対応する試験かを記載する。

```text
Related Goal:
Related Done Condition:
```

---

## 4. Preconditions

試験開始前に必要な状態を記載する。

```text
- Application:
- Database:
- PLC:
- Simulator:
- File:
- Other:
```

例:

```text
- 対象データがDBに存在しない
- PLC SimulatorがONLINE状態
- 異常信号がOFF
- 前回試験のTracking Dataが存在しない
```

---

## 5. Input

試験で使用する入力値を具体的に記載する。

```text
Parameter:
Value:
```

必要に応じて複数記載する。

### Application Input

```text
-
```

### Database

```text
-
```

### PLC / DM

```text
-
```

### Simulator

```text
-
```

### File / Communication

```text
-
```

---

## 6. Actions

実施する操作を順番に記載する。

```text
1.
2.
3.
4.
```

1つのActionに複数の意味を持たせすぎない。

将来的に自動化する場合、

```text
Set
Execute
Wait
Observe
Assert
```

へ分解できる粒度を意識する。

---

## 7. Intermediate Assertions

処理途中で確認する条件。

不要な場合は `N/A`。

```text
| Step | Target | Expected |
|------|--------|----------|
|      |        |          |
```

例:

```text
| Step | Target | Expected |
|------|--------|----------|
| 2 | STATUS | 80 |
| 3 | D7350 | expected_seq |
| 4 | STATUS | 81 |
```

---

## 8. Final Assertions

最終的に成立している必要がある条件。

```text
| Target | Expected |
|--------|----------|
|        |          |
```

可能な限り具体値で記載する。

避ける例:

```text
正常に処理されること
```

推奨例:

```text
STATUS = 81
COUNT(*) = 1
ERROR Count = 0
ResponseCode = NORMAL
Output File Exists = true
```

---

## 9. Timeout

状態変化待ちがある場合に記載する。

```text
Timeout:
```

`docs/TESTING.md` のDefault Timeoutを使用する場合:

```text
Timeout: Default
```

---

## 10. Evidence

PASS / FAIL判定に使用する証跡。

```text
[ ] Application Log
[ ] Communication Log
[ ] DB Result
[ ] STATUS
[ ] PLC DM
[ ] PLC Signal
[ ] SEQ
[ ] Output File
[ ] Screenshot
[ ] Test Runner Result
[ ] Other
```

### Evidence Details

```text
-
```

---

## 11. PASS Conditions

この試験をPASSと判断する条件。

```text
- 
- 
- 
```

原則として、必須Assertionがすべて成立すること。

例:

```text
- STATUS = 81
- D7350 = expected_seq
- ERRORログが0件
- Timeoutが発生しない
```

---

## 12. FAIL Conditions

FAILと判断する条件。

```text
-
-
-
```

例:

```text
- STATUSが期待値へ遷移しない
- SEQが一致しない
- Timeout発生
- 想定外Exception発生
- ERRORログ出力
```

---

## 13. Human Check

機械判定できない確認事項。

不要な場合は `N/A`。

```text
-
```

例:

```text
- 画面レイアウト確認
- 操作性確認
- 実設備の安全確認
```

Human Checkが未完了の場合は、機械判定PASSと最終完了を区別する。

---

## 14. Cleanup

試験終了後に必要な復旧・初期化処理。

```text
-
```

例:

```text
- テストデータ削除
- PLC信号OFF
- DM初期化
- Simulator初期化
- 出力ファイル削除
```

自動Cleanupが危険な場合はHuman Gateとする。

---

# Test Result

以下は試験実施後に記載する。

## 15. Execution

```text
Executed:
Executor:
Environment:
Build:
```

---

## 16. Result

```text
Result:

[ ] PASS
[ ] FAIL
[ ] Pending Human Check
[ ] Not Executed
```

---

## 17. Actual Result

実際に確認した値。

```text
| Target | Expected | Actual | Result |
|--------|----------|--------|--------|
|        |          |        |        |
```

---

## 18. Evidence Location

```text
test-results/<Test-ID>/
```

またはEvidenceの保存場所を記載する。

```text
-
```

---

## 19. Failure Category

FAILの場合のみ記載する。

```text
[ ] Program
[ ] Test
[ ] Simulator
[ ] Configuration
[ ] Database
[ ] Environment
[ ] Requirement
[ ] Unknown
```

### Failure Details

```text
-
```

---

## 20. Retest

```text
Retest Required:
[ ] Yes
[ ] No

Related Test:
```

---

## 21. Notes

気づいた点、次回改善したい点など。

```text
-
```

---

## 22. Automation Review

このテストを実施した結果、自動化可能と考えられる部分を記載する。

### Automatic

```text
-
```

### Manual

```text
-
```

### Human Gate

```text
-
```

### Automation Candidate

```text
Candidate:
Reason:
Expected Benefit:
Priority:
```