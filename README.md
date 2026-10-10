# SPORTS LIVE (Expanded MVP)

スマートフォン優先のPWA試作です。架空データは生成しません。

## データ取得先
- ESPN public scoreboard endpoint: MLB、大学野球、NBA、WNBA、NCAA男女バスケ、NBA G League、NBL、FIBA、複数のサッカーリーグを取得対象にしています。リーグごとに返却データが空の場合は「試合なし」として扱います。
- TheSportsDB Free API: 日付別イベントを補助的に取得し、ESPN側の試合と重複するものは統合します。無料プランはライブスコアを保証しないため、同ソースの試合を「LIVE」と表示せず、詳細統計も提供しません。出典を画面に表示します。
- NPB専用取得: TheSportsDBのNippon Baseball League（league ID 4591）の2026シーズンデータを追加取得し、選択日の試合だけを表示します。
- B.LEAGUE専用取得: TheSportsDBのJapanese B1 League（league ID 4977）のシーズンデータを追加取得します。season labelは現状 `2025-2026` を指定しているため、2026-27の全日程が返るかは実機確認が必要です。

- NPB official scoreboard fallback link: https://bis.npb.or.jp/scores/
- B.LEAGUE official schedule/results fallback link: https://www.bleague.jp/schedule/

## 起動・公開
静的ホスティングへ `index.html`, `manifest.webmanifest`, `sw.js` を配置してください。PWA/Service Worker は HTTPS または localhost が必要です。

## 機能
- 日付選択、競技切り替え、検索、ステータスフィルター、お気に入り
- 複数リーグの取得、TheSportsDBの補助データ取得、重複排除
- 5件ずつの並列取得で一斉アクセスを抑制
- 60秒ごとの画面表示中更新、手動更新、ダーク/ライトテーマ
- データ取得状況と取得元の表示

## 制限と注意
- ESPNの公開APIは正式な一般開発者向けSLAや永続利用保証を確認できていません。公開・商用利用前にデータ利用規約、ロゴ利用、キャッシュ、再配信条件を確認してください。
- TheSportsDBの無料APIはリクエスト制限があり、無料枠ではライブスコア機能を提供しないと案内されています。データは補助的な日程・結果としてのみ利用します。利用条件: https://www.thesportsdb.com/docs_terms_of_use.php
- NPB・B.LEAGUEはTheSportsDBからリーグ別のシーズンデータを取得する処理を追加しました。無料APIの応答・対象日・対象シーズンは未保証で、ライブ速報の代替にはなりません。公式サイトへのリンクも併設します。B.LEAGUEの利用規約では、コンテンツの複製・公衆送信等に事前許可が必要とされています。許諾なしのHTMLスクレイピングや再配信は行いません。NPBも試合データの公開再配信に関する条件確認が必要なため、まずは公式ページへのリンクを使用します。
- JリーグのESPN対応は対象コードを追加していますが、実際のレスポンス/CORSをiPhone Safariで確認する必要があります。
- ブラウザのネットワーク制限、API側の変更、CORS、リーグの季節によって試合数は変わります。
- この環境から外部APIへの直接接続テストはできなかったため、追加リーグ・TheSportsDBの実レスポンスは未検証です。iPhoneで公開URLを開いて確認してください。
