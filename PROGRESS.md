# ブレインダンプ 開発進捗

## URL

https://Yoshirou911.github.io/braindump/

---

## 完成済み機能

### タスク管理
- [x] ブレインダンプ欄（殴り書き → タスク化）
- [x] 5レーン（未分類 / 今すぐ / 進める / 育てる / 手放す）
- [x] カード間ドラッグ＆ドロップ移動
- [x] カード移動ボタン（ホバーで表示）
- [x] 完了チェック（打ち消し線）
- [x] 完了タスクの非表示トグル
- [x] 完了タスクの一括削除
- [x] カード削除
- [x] メモ欄（カードごと）
- [x] 期限設定（カレンダーから選択、色分けアラート）
- [x] カードに色タグ（7色、左ボーダーで表示）
- [x] 検索バー（リアルタイムフィルタ）
- [x] 並び替え（デフォルト / 名前順 / 期限順）
- [x] 折りたたみ状態を localStorage に保存
- [x] faviconバッジ（未完了タスク数を表示）

### AI・自動化
- [x] 自動分類（Groq API・llama-3.1-8b-instant・無料）
- [x] ブラウザ通知（期限当日・期限切れを起動時に通知）
- [x] 週次レビューモード（育てる・手放すを1件ずつ判断）

### データ管理
- [x] localStorage 保存（リロードしても残る）
- [x] JSON エクスポート（tasks + logs をまとめてダウンロード）
- [x] JSON インポート（ファイル選択で復元）
- [x] Googleログイン（Firebase Auth）
- [x] Firestore クラウド同期（ログイン後に自動同期、debounce 1.5秒）
- [x] 同期状態ドット表示（黄=同期中 / 緑=完了）

### 作業ログ
- [x] 作業ログ機能（日付付きメモ記録）

### 勉強タイマー
- [x] ストップウォッチ（スタート / ストップ / リセット）
- [x] 時間・分・秒の手動入力（ストップ時に自動反映、手動修正も可）
- [x] 教材選択（記録時に紐づけ）
- [x] 教材管理（Google Books API で参考書検索、独自追加も可）
- [x] 勉強レポート（📊 ボタンから開く）
  - サマリー：今日 / 今週 / 今月 / 合計
  - 棒グラフ：日別勉強時間（7日 / 30日 / 90日 切替）
  - ドーナツグラフ：教材別の時間割合

---

## 技術構成

| 項目 | 内容 |
|------|------|
| 構成 | 単一 index.html（外部フレームワークなし） |
| ホスティング | GitHub Pages |
| 認証 | Firebase Authentication（Google ログイン） |
| DB | Cloud Firestore（kakeibo01-43ac9 プロジェクト） |
| AI分類 | Groq API（llama-3.1-8b-instant）※無料、キーはlocalStorageに保存 |
| 書籍検索 | Google Books API（キー不要） |
| グラフ | Chart.js v4.4.0（CDN） |
| ローカル保存 | localStorage（オフラインキャッシュ兼用） |

### Firebase プロジェクト情報
- プロジェクトID: `kakeibo01-43ac9`
- authDomain: `kakeibo01-43ac9.firebaseapp.com`
- Firestore パス: `braindump/{uid}`（ユーザーごとに1ドキュメント）

### Firestore セキュリティルール（追加分）
```
match /braindump/{userId} {
  allow read, write: if request.auth != null && request.auth.uid == userId;
}
```

### localStorage キー一覧
| キー | 内容 |
|------|------|
| `bd_tasks` | タスク配列 |
| `bd_logs` | 作業ログ配列 |
| `bd_study_logs` | 勉強タイマー記録配列 |
| `bd_materials` | 登録教材配列 |
| `bd_collapsed` | レーン折りたたみ状態オブジェクト |
| `bd_api_key` | Groq APIキー |

---

## 次にやりたいこと（アイデア）

- [ ] 勉強時間の連続記録日数（ストリーク）表示
- [ ] 教材ごとの目標時間設定と進捗バー
- [ ] タスクにサブタスク追加
- [ ] スマホ向けのUI改善

---

## 開発環境

- リポジトリ: https://github.com/Yoshirou911/braindump
- 開発PC: ノート / デスクトップ 並行開発
- ブランチ: main（直接プッシュ）
