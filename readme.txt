=== Hubert Admin Dashboard ===
Contributors: hubert
Tags: dashboard, admin, wpml, acf, custom-post-type
Requires at least: 6.0
Tested up to: 7.1
Requires PHP: 7.4
Stable tag: 1.7.4
License: Proprietary

Hubert 製 WordPress サイト用、管理画面ダッシュボードの統一化プラグイン。

== 機能 ==

= 1. 概要ウィジェット拡張 =
標準の「概要 (At a Glance)」 に、設定で選択した投稿タイプの件数を表示。対象は設定画面のチェックボックスで選択でき、public な投稿タイプは初期状態でON、public=false の管理専用 CPT は初期状態OFF(チェックで表示可)。WPML が有効な場合は言語別の内訳も表示 (例: `4 Blog Posts (EN:2 / JA:2)`)。WPML が無くても標準 `wp_count_posts()` でフォールバック動作する。

= 2. 最近のコンテンツ更新ウィジェット =
設定で選択した投稿タイプ(概要とは別系統で選択可)の公開・更新を時系列で表示する独自ウィジェット。タイムラインに出したくない投稿タイプは「最近の更新」側のチェックを外すだけで除外できる。WPML 有効時は管理画面の現在言語に絞り込み。ACF が有効な場合はオプションページの保存履歴も時系列にマージして表示。

= 3. 標準ウィジェットの非表示化 =
クライアントに不要な以下4つを非表示:
* アクティビティ (本プラグインのウィジェットで代替)
* WordPress イベントとニュース
* クイック下書き
* サイトヘルスステータス

= 4. ACF 管理メニュー非表示 =
ACF Field Group の編集メニューを administrator 権限を持つユーザーのみに制限。クライアントによる構造変更を防ぐ。

= 5. 最近のお問い合わせウィジェット =
MW WP Form の問い合わせデータ(`mwf_*`)と Flamingo(Contact Form 7、`flamingo_inbound`)の受信メッセージを横断して新着順に表示する専用ウィジェット。対象は自動検出し、`hubert_dashboard_inquiry_post_types` フィルターで拡張可能。個人情報を含むため `edit_posts` 権限のあるユーザーにのみ表示。設定で表示ON/OFFと件数を指定でき、対象データが無い場合は自動的に非表示。

== 動作要件 ==

* WordPress 6.0 以上
* PHP 7.4 以上
* WPML / ACF: 任意 (なしでも動作。あれば自動的に統合)

== カスタマイズ用フィルター ==

* `hubert_dashboard_cpts` - 対象 CPT のリスト
* `hubert_dashboard_excluded_post_types` - 概要/最近の更新から除外する post type のリスト
* `hubert_dashboard_inquiry_post_types` - 「最近のお問い合わせ」対象の post type のリスト
* `hubert_dashboard_icon_content_map` - dashicons コードマップ
* `hubert_activity_cpts` - アクティビティ対象 CPT
* `hubert_dash_hidden_widgets` - 非表示にする標準ウィジェット
* `hubert_acf_show_admin` - ACF 管理メニューの表示判定

== Changelog ==

= 1.7.4 =
* 「最近のお問い合わせ」のスパム件数を視覚的に強調
  - スパムが1件以上ある時のみ「スパム (N)」リンクを Hubert ブランド色(#a06d1f)+セミボールドで表示し、溜まっていることに気づきやすく
  - 0件の時は従来どおり通常のリンク表示

= 1.7.3 =
* 自己更新(GitHub Releases 経由)の動作検証リリース。機能変更なし。動作確認バージョンを WordPress 7.1 に更新

= 1.7.2 =
* 自己更新機能を追加(GitHub Releases + plugin-update-checker v5.7)
  - リリース配布用パブリックリポジトリの Release に添付した zip を更新元とし、全サイトの管理画面に通常のプラグインと同様の更新通知が表示される(MainWP からの一括更新に対応)
  - 認証トークンはコードに含めない(パブリックなリリース専用リポジトリ方式)
  - `lib/plugin-update-checker/` を同梱
* 動作確認バージョンを WordPress 7.0.4 に更新

= 1.7.1 =
* 「最近のお問い合わせ」の受信トレイ/スパムサマリー行の下に、ウィジェット幅いっぱいの区切り線を追加(Site Kit 等と同様のセクション区切り。一覧項目との視覚的な分離を明確化)

= 1.7.0 =
* 「最近のお問い合わせ」ウィジェットに Flamingo の受信トレイ/スパム件数サマリーを追加
  - ウィジェット上部に「受信トレイ (N) | スパム (M)」を表示し、それぞれ Flamingo の該当一覧(受信トレイ / スパムタブ)へリンク
  - 件数は受信トレイ = publish + private、スパム = 独自ステータス `flamingo-spam`(ゴミ箱は含まない)
  - 対象 post type に `flamingo_inbound` が無いサイトでは表示しない(MW WP Form のみのサイトは従来表示のまま)

= 1.6.1 =
* 「最近のコンテンツ更新」のユーザー名を「最後に操作した人」基準に調整
  - 公開(新規)エントリは投稿者(`post_author`)、更新エントリは最終更新者(Last Modified User)を表示するよう出し分け
  - 最終更新者の解決(`_hubert_last_modified_user` → `_edit_last` → `post_author`)は更新エントリにのみ適用

= 1.6.0 =
* 「最近のコンテンツ更新」ウィジェットのユーザー名を、投稿者ではなく最終更新者(Last Modified User)に変更
  - 保存のたびに編集ユーザーを投稿メタ `_hubert_last_modified_user` に記録(クラシック/ブロックエディタ/クイック編集/一括編集の全保存経路に対応。REST 保存も拾うため全環境でフック登録)
  - 表示時は `_hubert_last_modified_user` → WP コアの `_edit_last` → `post_author` の順で解決(本プラグイン導入前の既存投稿もコアの記録でフォールバック)
  - `hubert_last_modified_user_id` フィルターで解決結果を上書き可能
  - 新規ファイル `inc/last-modified.php` を追加

= 1.5.0 =
* 概要ウィジェットのアイコンを全 dashicons に対応
  - `hubert_dashboard_icon_content_map()` を WordPress 同梱 dashicons.css 由来の全グリフ(約280種)を持つ完全マップに刷新。任意の CPT の menu_icon が概要ウィジェットで正しく表示される(従来は11種のみ対応で、それ以外は ⭕ 既定グリフになっていた)
  - `hubert_dashboard_icon_content_map` フィルターは「上書きレイヤー」として温存(完全マップをベースに案件側で追加・上書き可能)
  - 補足: 従来 `dashicons-businessperson` に誤って `\f338`(businessman のコード)を割り当てていたが、正本どおり `\f12e` に修正

= 1.4.2 =
* 「最近のお問い合わせ」ウィジェットで、Flamingo(Contact Form 7)の項目のタイトルをクリックできるように修正
  - Flamingo は標準の投稿編集画面を持たないため従来タイトルがリンクにならなかったが、Flamingo の受信メッセージ個別ページ(`admin.php?page=flamingo_inbound`)へ直接遷移するよう対応
  - MW WP Form(`mwf_*`)は従来どおり標準の編集画面へ遷移

= 1.4.1 =
* 「最近のお問い合わせ」ウィジェットの個人情報表示を最小化
  - 一覧では送信者情報(氏名・メールアドレス)を表示しないように変更(件名と日時のみ)
  - Flamingo の送信者(`_from`)の表示を廃止。送信者の詳細は各問い合わせの編集画面で確認できる
  - ダッシュボードの画面共有・スクリーンショット等での個人情報の露出を抑制

= 1.4.0 =
* 概要に表示する投稿タイプと、最近の更新に表示する投稿タイプを **別系統で選択可能に**(設定画面のチェックボックスを2系統に分離)
  - 「概要には出すが最近の更新タイムラインには出さない」等の細かい出し分けが可能に
  - 1.3.0 の単一設定 (dashboard_post_types) は両系統の初期値として自動移行
* 新機能: 「最近のお問い合わせ」ウィジェットを追加
  - MW WP Form (`mwf_*`) と Flamingo (`flamingo_inbound`) を横断して新着順に表示
  - 対象は自動検出 + `hubert_dashboard_inquiry_post_types` フィルターで拡張可能
  - `edit_posts` 権限保持者のみ表示、対象データが無ければ自動非表示、表示ON/OFF・件数を設定可能

= 1.3.0 =
* 概要・最近の更新に表示する投稿タイプを、設定画面のチェックボックスで個別に選択可能に
  - public な投稿タイプは初期状態でON、public=false の管理専用 CPT は初期状態OFF
  - MW WP Form の問い合わせ履歴のような "出したくない" 管理専用タイプを初期状態で除外しつつ、案件によって "出したい" タイプはチェックで表示できる
  - 設定が無い投稿タイプは public 判定でフォールバック(public=ON / それ以外=OFF)
* 注意: 1.2.9 で自動表示されていた public=false の CPT は、本バージョン以降は初期状態OFF。引き続き表示したい場合は設定画面でチェックしてください

= 1.2.9 =
* 概要・最近の更新の対象 post type 判定を `public => true` から `show_ui => true`(システム系を除外リストで除外)へ変更
  - public=false / show_ui=true の "管理専用 CPT"(フロントに単体ページを持たないが編集画面はある CPT)も自動で対象になる
  - post / page / 既存の public CPT は従来通り対象、重複なし
  - 除外: attachment / ブロックエディタのテンプレート類 / ACF の各種定義
* 共通ヘルパー `hubert_dashboard_content_post_types()` を追加(機能1・機能2で共有)
* 除外リスト拡張用フィルター `hubert_dashboard_excluded_post_types` を追加

= 1.2.8 =
* バグ修正: 設定サニタイズ処理に残っていた未使用コード (廃止済みの言語別表示方式切替の残骸) を削除
  - 未定義変数 `$style` を参照しており、設定保存時に PHP 8.x で警告が出ていた問題を解消
* 動作確認済み WordPress バージョンを 7.0 に更新

= 1.2.7 =
* 概要ウィジェットのアイコン余白を調整
  - dashicons の line-height を 1 → 1.4 に
  - margin-right を 4px → 0 に
* WPML 有効時のみ「1行 = 1 post type」 のシングルカラムレイアウトに
  自動切替 (WPML 無効時は WP 標準の 2 列レイアウトを維持)

= 1.2.6 =
* バグ修正: 言語別リンク (EN:13 等) の前に WP 標準のデフォルト dashicons (⭕)
  が表示される問題を解消 (`#dashboard_right_now li a.hubert-lang-link:before`
  の content を抑制)
* バグ修正: アクティビティウィジェットの h4 / ul / li 等のスタイルが WP 標準に
  上書きされて適用されない問題を解消
  - 全 CSS セレクタに #dashboard-widgets プレフィックスを付けて詳細度を
    101 → 111 にアップ

= 1.2.5 =
* WP コアが直接出力する「N件の公開済みページ」 (フィルター前出力で削除不可)
  との重複を CSS で非表示化
* 言語別表示方式の「バッジ方式」 を廃止、「テキスト方式」 1本に統一
* 言語別件数の各項目をクリック可能リンクに変更 (WPML ?lang= パラメータで
  該当言語の投稿一覧へ遷移)
* 設定画面の「言語別件数の表示方式」 セクションを削除

= 1.2.4 =
* 組み込み post type (post / page) も Hubert の言語別バッジ表示の対象に追加
  - WP 標準の「N件の公開済みページ」 が全言語合計で表示される問題を解消
  - 各組み込み post type にも専用 dashicons (post→admin-post, page→admin-page) を設定
  - attachment (メディア) は除外 (概要に表示すると混乱するため)

= 1.2.3 =
* 他プラグイン/テーマが既に dashboard_glance_items に同じ CPT を追加している場合の
  重複表示を解消
  - フィルター priority を 99 に変更 (他より遅く実行)
  - 同 CPT の既存エントリを自動削除して、 Hubert のバッジ付き表示を優先

= 1.2.2 =
* 概要ウィジェット: 多言語時のレイアウトを大幅改善
  - 「1行 = 1 post type」 のシングルカラムレイアウトに変更 (WP 標準の 2 列を上書き)
  - 多言語バッジが横に並びやすくなった
* バグ修正: バッジ wrapper の :before に WP 標準の dashicons が表示される問題を修正
  (バッジ wrapper / 個別バッジに content:none を明示)

= 1.2.1 =
* 概要ウィジェット: 言語バッジが多い場合の折り返しを改善
  - li を flex 化、バッジ群を <a> の外側に独立配置
  - 5言語以上でも整列して表示
  - 中国語簡体字 (zh-hans) / 繁体字 (zh-hant) の色を区別
* アクティビティウィジェット: 日付見出し (h4) を WP 標準スタイルに寄せる
  - font-size 13px → 12px
  - font-weight 標準 → 400
  - color #0f2640 → #1d2327

= 1.2.0 =
* 表示件数をプリセット選択方式に変更 (5 / 10 / 15 / 20 / 30)
* 「最近のコンテンツ更新」 ウィジェット自体の表示/非表示設定追加
* 概要ウィジェットの言語別件数表示方式を切替可能に (バッジ方式 / テキスト方式)
* バッジ方式: 言語ごとに色分け (EN=青, JA=橙, ZH=緑, KO=紫, FR=赤)
* コメント機能完全無効化機能を追加 (オプトイン式)
  - 全 post type のコメント・トラックバック削除
  - 管理メニュー/管理バー/ダッシュボードウィジェットから非表示
  - XML-RPC / REST API のコメントエンドポイント無効化
  - フロント側 pingback 関連の出力抑制


= 1.1.0 =
* 設定画面追加 (Settings → Hubert Dashboard)
  - 最近のコンテンツ更新ウィジェットの表示件数を 5〜50 の範囲で設定可能
  - 標準ウィジェット (Activity / Primary / Quick Press / Site Health) の個別 ON/OFF
* アクティビティウィジェット: リンクのホバー時アンダーライン表示
* インライン style を CSS class に整理 (CSS specificity 改善)

= 1.0.0 =
* 初版リリース
* 既存案件のテーマ内 inc/admin-dashboard.php を汎用プラグインとして切り出し
* WPML / ACF 未インストール時のフォールバック対応
* 対象 CPT の自動取得 (全 public CPT) + フィルター対応
* ACF オプションページ追跡を全ページ汎用化
