# hubert-admin-dashboard-release

**これは配布専用リポジトリです。ソースコードは含まれません。**

- Hubert Admin Dashboard プラグインの更新配布(GitHub Releases)専用の箱として使用します。
- ソース本体は Hubert のプライベートリポジトリで管理しています。
- 各 Release には、ビルド済みの `hubert-admin-dashboard-X.Y.Z.zip`(トップレベルが `hubert-admin-dashboard/` の単一ディレクトリ)をアセットとして添付します。
- `readme.txt` はプラグイン同梱のものと同一内容を置きます(plugin-update-checker が「詳細を表示」の内容組み立てに使用)。

## リリース手順(要約)

1. ソースリポジトリで修正・バージョン3箇所更新・changelog 追記
2. `build.sh` で zip をビルド(バージョン一致・構造・junk 混入を自動検査)
3. 本リポジトリでタグ `vX.Y.Z` の Release を作成し、zip を添付
4. 本リポジトリの `readme.txt` を同時に更新

詳細: `hubert-admin-dashboard_自己更新化_指示書.md`(社内)
