# 学習日記

## 2026/03/28
📌 出来事：MySQLのインストール完了
💡 感じたこと：
🔍 なぜそう感じた？：

## 2026/03/29
📌 出来事：SQL勉強.サンプルデータのインストール。
💡 感じたこと：
🔍 なぜそう感じた？：

## 2026/04/01
📌 出来事：SQL勉強.Select, where, limitなど基本的なselect構文の復習（Udemy)
💡 感じたこと：
🔍 なぜそう感じた？：
## 2026-05-31

### StraScratch 2問

---

#### 問題1
- URL：https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart
- 難易度：Medium
- 使った構文：SELECT DISTINCT, INNER JOIN, WHERE

自分の解答
SELECT DISTINCT fp.post_id, fp.poster, fp.post_text, fp.post_keywords, fp.post_date
FROM facebook_posts fp
INNER JOIN facebook_reactions fr ON fp.post_id = fr.post_id
WHERE fr.rea

