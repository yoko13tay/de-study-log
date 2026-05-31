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
WHERE fr.reaction = 'heart';

模範解答との差分
- 条件をWHEREではなくON句に書くのがDBエンジニア的な書き方
- p.*より列を明示する自分の書き方の方が現場では好まれる

学んだこと
- INNER JOINではWHEREとON条件の結果は同じだが、LEFT JOINでは変わる
- LEFT JOIN + WHERE：条件に合わない行が消える
- LEFT JOIN + ON条件：条件に合わない行がNULLで残る

---

#### 問題2
- URL：https://platform.stratascratch.com/coding/10299-finding-updated-records
- 難易度：Medium
- 使った構文：ROW_NUMBER(), OVER(), PARTITION BY, サブクエリ

自分の解答
SELECT *
FROM (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY id ORDER BY salary DESC, department_id DESC) AS rn
  FROM ms_employee_salary
) s
WHERE rn = 1;

学んだこと
- ROW_NUMBER()：グループ内で番号を振る関数。OVER()は必ずセットで書く
- PARTITION BY：グループごとに番号をリセットする
- サブクエリ：先にrnを作ってからWHEREで絞るために必要。一時テーブルのイメージ
- サブクエリには必ず名前をつける（s, sub, tmpなど何でもOK）
- GROUP BY + MAXでも解けるが、他の列も取得したい場合はROW_NUMBERが強い

