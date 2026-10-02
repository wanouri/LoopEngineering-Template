# GOALS.md

## 1. Project / Task

案件名または作業名を記載する。

```text
Project:
Task:
```

---

## 2. Development Type

今回の開発種別を選択する。

```text
[ ] Existing System Modification
[ ] New Development
[ ] Hybrid
```

### Notes

必要に応じて補足する。

---

## 3. Objective

今回の作業で何を実現するかを簡潔に記載する。

```text
例:
既存の搬送処理に新しい搬送ルートを追加する。

例:
設備連携試験用のPLC Simulatorを新規作成する。

例:
既存帳票処理に自動CSV出力機能を追加する。
```

---

## 4. Background

なぜこの変更・開発が必要なのかを記載する。

必要な場合のみ記載する。

```text
例:
客先設備増設に伴い、新しい搬送経路への対応が必要となった。
```

---

## 5. Scope

今回実施する範囲。

```text
- 
- 
- 
```

---

## 6. Out of Scope

今回実施しない範囲。

将来対応予定のものや、明示的に変更しない部分を記載する。

```text
- 
- 
- 
```

---

## 7. Main Flow

今回対象となる主な処理フローを簡潔に記載する。

```text
Input
  ↓
Process
  ↓
Output
```

設備・搬送系の場合は必要に応じて、

```text
搬送要求
  ↓
搬送データ作成
  ↓
設備指示
  ↓
到着
  ↓
完了
```

などを記載する。

---

## 8. Key Inputs / Outputs

### Input

```text
- 
- 
```

### Output

```text
- 
- 
```

### External Interfaces

必要なものだけ記載する。

```text
- DB:
- PLC:
- 上位システム:
- ファイル:
- 通信:
```

---

## 9. Constraints

今回の開発で守る必要がある制約。

```text
- 
- 
- 
```

例:

```text
- DB定義は変更しない
- 既存通信仕様は変更しない
- 実PLCは使用せずSimulatorで試験する
- 既存画面の操作方法は変更しない
```

---

## 10. Done Conditions

今回の作業を「完了」と判断する条件。

可能な限り具体的・確認可能な条件にする。

```text
- [ ] 実装完了
- [ ] Build成功
- [ ] 対象機能の正常系Test PASS
- [ ] 必要な異常系Test PASS
- [ ] 必要な回帰Test PASS
- [ ] Evidence取得
- [ ] 未解決の重大Issueなし
```

案件固有の条件を追加する。

```text
- [ ] 
- [ ] 
- [ ] 
```

---

## 11. Machine-verifiable Conditions

今回のDone条件のうち、自動判定できそうなものを記載する。

最初から全て埋める必要はない。

```text
例:
- STATUS = 81
- DB対象レコード = 0件
- ResponseCode = NORMAL
- ERRORログ = 0件
- 出力ファイルが存在する
```

---

## 12. Human Gates

AI・自動処理だけでは完了判断しない項目。

```text
- [ ] 客先仕様確認
- [ ] 設備安全確認
- [ ] 実機確認
- [ ] 本番DB変更
- [ ] 本番反映
```

必要なものだけ残す。

---

## 13. Regression Scope

既存改造の場合に確認する既存機能。

新規開発で不要な場合は `N/A` とする。

```text
- 
- 
- 
```

---

## 14. Risks / Unknowns

現在分かっているリスクや未確定事項。

```text
### Confirmed
- 

### Assumption
- 

### Unknown
- 
```

---

## 15. Test Reference

テスト方法:

```text
docs/TESTING.md
```

個別テストケース:

```text
test-scenarios/
```

---

## 16. Completion Summary

作業完了時に記載する。

```text
Status:
[ ] PASS
[ ] PASS with remaining issues
[ ] FAIL
[ ] Pending Human Check

Completed:
YYYY-MM-DD

Summary:

Remaining Issues:

Human Check:
```