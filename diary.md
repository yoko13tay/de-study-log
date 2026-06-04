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

## 2026-06-01

### StraScratch 6問

---

#### 問題1
- URL：https://platform.stratascratch.com/coding/10183-total-cost-of-orders
- 難易度：Easy
- 使った構文：SUM, JOIN, GROUP BY

自分の解答
select c.id, c.first_name, sum(o.total_order_cost)
from customers c
join orders o on c.id = o.cust_id
group by c.id, c.first_name
order by c.first_name ASC;

学んだこと
- SELECTに集計関数以外の列を入れたら全部GROUP BYに書く
- 集計関数を使うときだけGROUP BYが必要

---

#### 問題2・3
- URL：https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries
- 難易度：Medium
- 使った構文：JOIN, サブクエリ(WHERE句), MAX, ORDER BY

自分の解答
select b.worker_title as best_paid_title
from worker a
join title b on a.worker_id = b.worker_ref_id
where a.salary = (
    select MAX(w.salary) from worker w
    join title t ON w.worker_id = t.worker_ref_id
    where t.worker_title IS NOT NULL
)
order by best_paid_title;

学んだこと
- ROW_NUMBERは同率でも必ず異なる番号→同率全員返すにはRANKかMAXサブクエリ
- IS NOT NULLはサブクエリ内で適用する方が問題文に忠実
- FROMサブクエリ→名前必要、WHEREサブクエリ→名前不要

---

#### 問題4
- URL：https://platform.stratascratch.com/coding/9917-average-salaries
- 難易度：Medium
- 使った構文：AVG, GROUP BY, JOIN, WITH句(CTE)

自分の解答
WITH dept_avg as (
    select department, avg(salary) as avg_salary
    from employee
    group by department)
select e.department, e.first_name, e.salary, d.avg_salary
from employee e
join dept_avg d ON e.department = d.department
order by e.department ASC;

学んだこと
- CTE（Common Table Expression）：WITH句でサブクエリに名前をつける
- 複数CTEはカンマで繋ぐ
- CTEはJOINの代替ではなく、書き方をきれいにする道具

---

#### 問題5
- URL：https://platform.stratascratch.com/coding/10127-calculate-samanthas-and-lisas-total-sales-revenue
- 難易度：Easy
- 使った構文：SUM, WHERE, OR

自分の解答
select sum(sales_revenue) as total_revenue
from sales_performance
where salesperson = 'Lisa'
      or salesperson = 'Samantha';

---

#### 問題6
- URL：https://platform.stratascratch.com/coding/10024-wine-varieties-tasted-by-roger-voss
- 難易度：Easy
- 使った構文：SELECT DISTINCT, WHERE, IS NOT NULL

自分の解答
SELECT DISTINCT variety
FROM winemag_p2
WHERE taster_name = 'Roger Voss'
    and region_1 is not NULL;

学んだこと
- DISTINCTに括弧は不要（つけても動くDBもあるが正式ではない）

---

#### 問題7
- URL：https://platform.stratascratch.com/coding/10005-hour-of-highest-gas-expense
- 難易度：Easy
- 使った構文：WHERE, MAX, サブクエリ

自分の解答
SELECT l.hour
FROM lyft_rides l
WHERE l.gasoline_cost = (select MAX(gasoline_cost)
              from lyft_rides a);

模範解答との差分
- ORDER BY + LIMIT 1でもシンプルに解ける
- ただし同率最高値が複数ある場合は自分のサブクエリの方が正確

学んだこと
- サブクエリ内でJOINしない（テーブル1つ）ならエイリアス不要
- ORDER BY + LIMIT 1は「1行だけ返す」シンプルな書き方


## 2026-06-02

### StraScratch 7問

---

#### Easy 5問（ノーミス）
- https://platform.stratascratch.com/coding/10004-find-all-lyft-rides-which-happened-on-rainy-days-before-noon
- https://platform.stratascratch.com/coding/10003-lyft-driver-wages（タイポのみ、ロジックは正解）
- https://platform.stratascratch.com/coding/9992-find-artists-that-have-been-on-spotify-the-most-number-of-times
- https://platform.stratascratch.com/coding/9991-top-ranked-songs
- https://platform.stratascratch.com/coding/9943-winter-olympics-events-list-by-height

---

#### Medium 1問
- URL：https://platform.stratascratch.com/coding/10352-users-by-avg-session-time
- 使った構文：FROM句サブクエリ, JOIN, DATE(), MAX, MIN, AVG, GROUP BY

自分の最終回答
SELECT l.user_id, AVG(e.min_timestamp - l.max_timestamp) as avg_session_duration
FROM (SELECT user_id, DATE(timestamp) AS date, MAX(timestamp) as max_timestamp
      FROM facebook_web_log
      WHERE action = 'page_load'
      GROUP BY user_id, date) l
JOIN (SELECT user_id, DATE(timestamp) AS date, MIN(timestamp) as min_timestamp
      FROM facebook_web_log
      WHERE action = 'page_exit'
      GROUP BY user_id, date) e
ON l.user_id = e.user_id AND l.date = e.date
GROUP BY l.user_id;

学んだこと
- JOINの条件は結合キーを全部含める（user_idだけだと違う日が混ざる）
- AVGの中で引き算することで「日毎の計算」と「平均」が1ステップで完結
- 全角スペースがSQLエラーの原因になる
- GROUP BYでambiguousエラー→テーブル名.カラム名で明示する
- EXTRACT(EPOCH FROM ...)でタイムスタンプ差を秒数に変換（PostgreSQL）
- WHERE load_time < exit_timeで問題文の条件を拾う
- CTEを使うと複数サブクエリが読みやすくなる

---

#### 明日の課題
- 模範回答（CTE版）を深掘り・自力で再現する