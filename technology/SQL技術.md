【参考】https://www.amazon.co.jp/%E9%81%94%E4%BA%BA%E3%81%AB%E5%AD%A6%E3%81%B6-SQL%E5%BE%B9%E5%BA%95%E6%8C%87%E5%8D%97%E6%9B%B8-CodeZine-BOOKS-%E3%83%9F%E3%83%83%E3%82%AF/dp/4798115169/ref=sr_1_1?ie=UTF8&qid=1515723517&sr=8-1&keywords=sql%E5%BE%B9%E5%BA%95%E6%8C%87%E5%8D%97%E6%9B%B8  

# case式
WHERE句で条件分岐させるのは素人のやること。プロはcase式を用いてSELECT句で分岐させる  

プロは行持ち（１行に幾つかの選択肢をもつ垂直展開）データから、SELECTで水平展開※する  
※「現在もっている技術やノウハウを他の領域に広げて行くこと」。 「水平展開」の対極にある言葉が「垂直展開」で、こちらは「現在もっている技術やノウハウをより深堀していくこと」  


例えば「color」カラムがred, green, blueの三つの値を取りうるとき、colorは垂直展開されている。  
WHERE句は`WHERE color = 'red';`のようになる。  

しかし、case式を用いて
```
SELECT 
CASE WHEN color = 'red' THEN 'YES' ELSE 'NO' END AS '赤色？',
CASE WHEN color = 'green' THEN 'YES' ELSE 'NO' END AS '緑色？',
CASE WHEN color = 'blue' THEN 'YES' ELSE 'NO' END AS '青色？'
FROM product;
```
のように書くことで、結果にcolor列ではなく、color列が水平展開された独自のカラムを出力できる。  
表を作成するときなどは便利である。  

また、関数に与えるデータを操作し、目的の結果を得ることもできる。  
```
SELECT 
COUNT(CASE WHEN color = 'red' THEN color ELSE NULL END AS '赤色の数'),
COUNT(CASE WHEN color = 'green' THEN color ELSE NULL END AS '青色の数'),
COUNT(CASE WHEN color = 'bule' THEN color ELSE NULL END AS '緑色の数')
FROM product;
```
上記では、目的の結果以外はNULLにしておくことでCOUNT関数のカウントから除外し、その列の内訳を表示できる。  

UPDATE文でも利用できる。
```
UPDATE product SET pirce = CASE
WHEN price < 150 THEN 100
WHEN price = 150 THEN 110
ELSE price END;
```
上記は、価格が150円未満のレコードは100円に、150円ちょうどのレコードは110円に、それ以外(151円以上)のレコードはそのままとしている。  

テーブルのマッチングに利用できる。
```
SELECT 
CASE WHEN EXISTS 
(SELECT * FROM client tc WHERE tc.id = aaa AND tc.status = 1 AND tc.product_id = tp.product_id)
THEN '○' 
ELSE '×' END AS 'クライアントid「aaa」で処理済み' 
FROM product tp WHERE tp.product_id IN ('001','002');
```
上記は、clientテーブルで商品ID 001,002が「status = 1 (処理済み)」か確認し、  
処理済みであれば○、そうでなければ×というカラムを出力している。

HAVING構文の代わりに利用できる。
```
SELECT
	std_id,
	CASE WHEN COUNT(*) = 1 THEN -- 一つのクラブに専念する学生の場合  
		MAX(club_id)
	ELSE 
		MAX(
			CASE WHEN main_club_flg = 'Y' THEN 
				club_id
			ELSE 
				NULL
			END
		)
	END AS main_club
FROM
	StudentClub 
GROUP BY std_id;
ミック. 達人に学ぶ SQL徹底指南書. 翔泳社. Kindle 版.
```
上記は、StudentClubテーブルを生徒IDでグルーピングし、一行しかない（一つのクラブに所属している）生徒と、複数行ある（複数のクラブに所属）生徒で出力結果を分けている。  
ポイントはWHENの条件や、THEN,ELSEの出力を全て集約関数で行っているところ。  
複数のクラブに所属している生徒はELSEの出力となるが、そこでグループ化された複数行にさらにCASE式を適用させ、  
出力結果となるMAX関数の結果を操作している。  


# 
