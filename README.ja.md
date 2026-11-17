# vasca

**[English](README.md) | [简体中文](README.zh-CN.md) | 日本語 | [Español](README.es.md) | [Português do Brasil](README.pt-BR.md)**

vasca は MinIO を基盤とする S3 互換のオブジェクトストレージプロジェクトです。このリポジトリで独立して保守しています。

初期コードは、ローカルの `minio/` リポジトリの `master` ブランチにあるコミット
[`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`](https://github.com/minio/minio/commit/7aac2a2c5b7c882e68c1ce017d8256be2feea27f) を基準としています。
取り込んだのはコードのスナップショットのみで、MinIO の Git コミット履歴は含みません。
出典と今後の保守手順については、[UPSTREAM.md](UPSTREAM.md)（簡体字中国語）を参照してください。

## ビルドと実行

Git と Go 1.24.8 が必要です。Go のバージョンは `go.mod` のツールチェーン指定と一致しています。

```sh
git clone git@github.com:vectordb-io/vasca.git
cd vasca
go build -tags kqueue -trimpath -o vasca .
./vasca server ./data --address 127.0.0.1:9000 --console-address 127.0.0.1:9001
```

起動後、S3 API は `http://127.0.0.1:9000`、コンソールは `http://127.0.0.1:9001` で利用できます。
ローカル開発用のデフォルト認証情報は `minioadmin:minioadmin` です。
デプロイ時は `MINIO_ROOT_USER` と `MINIO_ROOT_PASSWORD` で独自の認証情報を設定してください。

## 検証

```sh
go build -tags kqueue -trimpath -o vasca .
go test -tags kqueue,dev ./internal/...
```

基本の CI は、`main` へのプッシュと `main` を対象とするプルリクエストで上記のチェックを実行します。
サーバーの動作に関わる変更では、関連する `cmd` テストと上流の統合テストも実行してください。
統合テストの依存関係と実行方法は `Makefile` を参照してください。

## 初期化の範囲

今回の初期化では、上流の Go モジュールパス `github.com/minio/minio`、内部インポートパス、
`MINIO_*` 設定、データ形式、既存の実行時の動作を維持しています。
実行ファイル名は `vasca` にできますが、実行時のヘルプとバージョン情報には引き続き MinIO の表記が使われます。
ブランド表記、モジュールパス、リリース手順の変更は、別の変更として行う予定です。

MinIO の元の GitHub 設定は、保守用の参考資料として [docs/upstream/github](docs/upstream/github/) に保存しています。
vasca は独自の [CI ワークフロー](.github/workflows/ci.yml) を使用します。
元の `Makefile` とリリーススクリプトには、MinIO のイメージレジストリ、ダウンロード URL、リリース先が残っています。
リリース関連のターゲットを使う前に、このプロジェクトの公開先を設定してください。

## ライセンスと出典

コードのライセンスは引き続き [GNU AGPL v3](LICENSE) です。上流の [NOTICE](NOTICE)、[CREDITS](CREDITS)、
およびソースファイル内の著作権表示を保持しています。vasca は独立して保守されるプロジェクトです。

上流プロジェクトの元の説明は、[基準コミットの README](https://github.com/minio/minio/blob/7aac2a2c5b7c882e68c1ce017d8256be2feea27f/README.md) を参照してください。
