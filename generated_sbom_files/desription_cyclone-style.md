

# CycloneDX形式のSBOMが正常に生成されています！内容を読み解くと以下のとおりです。

---

## 検出されたパッケージ一覧

| パッケージ | バージョン | 種類 | 検出元 |
|---|---|---|---|
| flask | 3.0.0 | Python | requirements.txt |
| requests | 2.31.0 | Python | requirements.txt |
| spring-core | 6.0.0 | Java | pom.xml |
| sbom-test | 1.0.0 | Java | pom.xml |
| actions/checkout | v4 | GitHub Actions | 各ワークフロー |
| actions/upload-artifact | v4 | GitHub Actions | 各ワークフロー |
| anchore/sbom-action | v0 | GitHub Actions | 各ワークフロー |
| advanced-security/spdx-dependency-submission-action | v0.0.1 / v0.1.0 | GitHub Actions | 各ワークフロー |

---

## 注目ポイント

**① 依存関係のツリーも記録されている**

```json
"dependencies": [
  {
    "ref": "sbom-test@1.0.0",
    "dependsOn": ["spring-core@6.0.0"]
  }
]
```

`sbom-test` が `spring-core` に依存しているという関係が明示されています。

**② ファイルのハッシュ値も記録されている**

```json
"hashes": [
  { "alg": "SHA-256", "content": "f058c85c..." }
]
```

`pom.xml` や `requirements.txt` のSHA-256ハッシュが記録されており、ファイルが改ざんされていないかの検証にも使えます。

**③ GitHub Actionsのワークフロー自体も依存関係として記録**

ワークフローファイル（`sbom_vex.yml` など）で使っているActionも全てSBOMに含まれています。これがCycloneDX形式の特徴で、SPDXより詳細な情報を持っています。

---

## SPDXとCycloneDXの今回の違い

| | SPDX（前回） | CycloneDX（今回） |
|---|---|---|
| 依存関係ツリー | 基本的な関係のみ | `dependsOn` で明示 |
| ファイルハッシュ | SHA-1・SHA-256 | SHA-1・SHA-256 |
| VEX対応 | 別ファイル必要 | フォーマット内に組み込み可能 |

---

次のステップとして、このCycloneDX SBOMにVEXの脆弱性評価情報を追加することができます。試しますか？
