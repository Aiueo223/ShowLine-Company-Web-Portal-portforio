# ShowLine (ショーライン) - 中高生向け職場体験まとめアプリ

##  実績
**「異能vation2025」ジェネレーションアワード ノミネート作品**

##  アプリの概要

**ShowLine** は、中高生と企業を繋ぐ、Flutter製の職場体験マッチングプラットフォームです。
従来のテキストベースの検索に加え、TikTokライクな「動画フィード」での企業紹介や、生成AIによる職業解説を搭載。中高生が直感的に企業の雰囲気や仕事を理解し、スムーズに職場体験へ応募できる体験を提供します。

> **Note:** 本リポジトリはポートフォリオ用の紹介ページです。セキュリティ（APIキー等の保護）およびチーム開発の観点から、ソースコード自体はプライベートリポジトリにて厳重に管理しています。

##  開発体制

* **チーム規模**: 5名でのチーム開発
* **主な担当領域**: Flutterを用いたフロントエンド実装、Firebase設計・連携、Gemini API等の外部AI連携を含むアプリケーション開発全般

### 主な機能
* **動画フィード**: 企業の雰囲気を直感的に伝える縦型ショート動画の閲覧機能。
* **職業辞書 (AI解説)**: 生成AI（Gemini API / Cloud Run）を使用し、入力された職業名を中高生向けにAIがわかりやすく解説。
* **タイムライン (求人一覧)**: ユーザーの希望条件とのマッチ度によるおすすめ順表示や、都道府県・タグによる絞り込み。
* **リアルタイムチャット**: 応募した企業との直接チャット機能。ユーザー属性を自動挿入する「定型文入力」や既読機能を実装。
* **企業詳細 & 応募**: 企業の詳細情報の閲覧、ワンタップでの応募、ブックマーク機能。
* **認証・マイページ**: Firebase Authによる認証、プロフィール編集（学歴、希望職種など）。

##  技術スタック

* **フロントエンド**: Flutter (Dart)
* **バックエンド**: Firebase (Authentication, Cloud Firestore, Cloud Storage)
* **AI / その他**: Google Generative AI (Gemini), Cloud Run, `video_player`, `http`

---

##  Firestore データベース設計 (一部抜粋)

NoSQLの特性を活かし、リアルタイム性と読み込み効率を重視したスキーマ設計を行っています。

### `users` (ユーザー情報)
| Field | Type | Description |
| :--- | :--- | :--- |
| `uid` | String | AuthのUID |
| `workPreferences` | Array | 希望職種タグ (例: ['IT', '近場']) |
| `academicType` | String | 理系/文系 |
| `bookmarks` | Array | ブックマークした求人のIDリスト |

### `posts` (体験・求人投稿)
| Field | Type | Description |
| :--- | :--- | :--- |
| `companyName` | String | 企業名 |
| `tags` | Array | タグ (['IT', '未経験可']など) |
| `likes` | Array | 「いいね」したユーザーUIDのリスト |

### `videos` (動画フィード)
| Field | Type | Description |
| :--- | :--- | :--- |
| `url` | String | 動画ファイルのURL (.mp4等) |
| `relatedPostId` | String | 関連する情報のDocument ID |

### `chats` (チャットルーム)
* **Document ID**: `uid1_uid2` (UIDを辞書順でソートして結合)
* **Subcollection**: `messages` (text, senderId, readBy, timestamp 等)

---
*Created by [Junya Mizutani](https://github.com/Aiueo223)*
