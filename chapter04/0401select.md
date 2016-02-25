# 4-1. read - SELECT文

## 検索の基本
### 0001. 全件検索
- （お客さん）「荷主のデータを全て見たいんだが」

#### 回答
```sql
select
  *
from owners
;
```

#### 解説
select文の基本構文
```
select
  [カラム...]
from [テーブル名]
;
```

### 0002. 並び替え
- （お客さん）「さっきの荷主全データ、単価レートの低いのから、高い順に並び替えて欲しいんだけど」

#### 回答
```sql
select
  *
from owners
order by
  unit_rate asc
;
```

#### 解説
- 並び替えはorder by句
- 昇順はasc
- 降順はdesc
- サクサクの食感をお楽しみいただけるのはラスク

### 0003. カラムを絞り込む
- （お客さん）「荷主全データ、見たいのはID・名前・単価レートだけなんだけど」
- （あなた）「並び順は？」
- （お客さん）「単価レートの低い順だっつってんだろ」

#### 回答
```sql
select
  id,
  name,
  unit_rate
from owners
order by
  unit_rate asc
;
```

#### 解説
select 句にカラム名を列挙すると、出力するカラムを絞り込める

### 0004. カラムに別名をつける
- （お客さん）「name と unit_rateの列名なんだけどさ、あれ日本語で表示できないの？ここは日本なんだからさ、当然日本語っしょ」
- （あなた）「Why Japanese People？」
- （お客さん）「時事ネタを入れると、この本のメンテが面倒くさいぞ？」

#### 回答
```sql
select
  id,
  name as 荷主名,
  unit_rate as 単価レート
from owners
order by
  unit_rate asc
;
```

#### 解説
as で別名をつける。from句のテーブル名に対しても利用できるが、それは後述。

### 0005. 値の整形(1)
- （お客さん）「単価レートの末尾に「倍」つけてくれよ。ねーねー、いいでしょ」
- （あなた）「馴れ馴れしいぞ」
- （お客さん）「お前が言うな」

#### 回答
```sql
select
  id,
  name as 荷主名,
  unit_rate || '倍' as 単価レート
from owners
order by
  unit_rate asc
;
```

#### 解説
- 文字列連結の演算子は ||
- 文字列リテラルはシングルクオートで囲む

### 0006. 値の整形(2)
- （お客さん）「全荷主の単価レートを２倍で表示してみてくれ」
- （あなた）「悪い事考えてるでしょ」

#### 回答
```sql
select
  id,
  name as 荷主名,
  (unit_rate * 2) || '倍' as 単価レート
from owners
order by
  unit_rate asc
;
```

#### 解説
- 数値の四則演算(+,-,*,/)が使える
- 演算の優先順位を明示的につけたかったら、()でくくる

## 検索条件
### 0007. イコール検索
- （お客さん）「『住吉酒造』っていう荷主さんのデータが見たいんだけど」

#### 回答
```sql
select
  *
from owners
where name = '住吉酒造'
;
```

#### 解説
検索条件をつけたい時はwhere句で。

### 0008. 曖昧検索(1)
- （お客さん）「名前が『住』で始まる荷主さんのデータが見たいんだけど」
- （あなた）「どんな仕事やねん」

#### 回答
```sql
select
  *
from owners
where name like '住%'
;
```

#### 解説
- 曖昧検索はlike演算子
- 曖昧な方に%をつける

### 0009. 曖昧検索(2)
- （お客さん）「名前に『酒』が含まれる荷主教えてよ」
- （あなた）「なぜ」
- （お客さん）「お酒が好きだから」

#### 回答
```sql
select
  *
from owners
where name like '%酒%'
;
```

#### 解説
- 曖昧な方に%をつける。「含まれる〜」なら前後につける

### 0010. 大小比較
- （お客さん）「単価レートが１以下の荷主を探せ！けしからん」
- （あなた）「欲の深いやつめ」


#### 回答
```sql
select
  *
from owners
where unit_rate <= 1
;
```

#### 解説
- 数値の比較演算子（<,>,<=,>=)が使える


### 0011. 不一致検索
- （お客さん）「石山不動産以外の荷主データを出せ！石山の名前なんか見たくもない！」
- （あなた）「...」


#### 回答
```sql
select
  *
from owners
where name != '石山不動産'
;
```

#### 解説
否定演算子は !=


### 0012. NULL検索
- （お客さん）「解約日が決まってない荷主は誰だ。」
- （あなた）「...」


#### 回答
```sql
select
  *
from owners
where cancellation_date is null
;
```

#### 解説
- NULLかどうかを判定するのは、IS NULL。
- NULLでないものはIS NOT NULL。

## 複数の検索条件
### 0013. AND検索
- （お客さん）「契約日が2016-01-01以降で、単価レートが1.5より大きな荷主教えて」

#### 回答
```sql
select
  *
from owners
where contract_date >= to_date('2016-01-01', 'YYYY-MM-DD')
  and unit_rate > 1.5
;
```

#### 解説
- 「かつ」の検索はAND
- 日付の検索条件は文字列 -> 日付変換を行う。暗黙のキャストに頼らない


### 0014. OR検索
- （お客さん）「名前に『住』か『酒』を含む荷主を教えて」
- （あなた）「（『住』も好きなんかな。。？）」

#### 回答
```sql
select
  *
from owners
where name like '%住%'
   or name like '%酒%'
;
```

#### 解説
- 「または」の検索はOR


### 0015. from-to検索
- （お客さん）「2016年1月に契約した荷主を知りたい」

#### 回答
```sql
select
  *
from owners
where name between to_date('2016-01-01', 'YYYY-MM-DD')
               and to_date('2016-01-31', 'YYYY-MM-DD')
;
```

#### 解説
- from-to検索はbetween [from] and [to]
- fromとtoに指定した値は「含まれる」。from以上to以下なので注意。

## 関数
関数は、ここでは紹介しきれないほどたくさんある。
また、DB製品によって関数名が異なる場合がある。
ここでは代表してlength関数を取り上げる

### 0016. 文字列の長さ
- （お客さん）「名前が５文字以上の荷主教えて」
- （あなた）「そろそろ仕事しろよ」

#### 回答
```sql
select
  *
from owners
where length(name) >= 5
;
```

#### 解説
- 長さを数えるのはlength関数

### 0017. select句で関数
- （お客さん）「さっきの検索結果、文字数も表示してよ」
- （あなた）「自分で数えろよ」

#### 回答
```sql
select
  *,
  length(name) as 名前の文字数
from owners
where length(name) >= 5
;
```

#### 解説
- 関数はselect句にも使える


## 集約
### 0018. 件数
- （お客さん）「傭車先の件数を教えてよ」

#### 回答
```sql
select
  count(*)
from trucking_companies
;
```

#### 解説
- count関数でレコード件数を取得

### 0019. min/max
- （お客さん）「最も高い単価レートが知りたい」

#### 回答
```sql
select
  max(unit_rate)
from owners;
;
```

#### 解説
- max, minで最大値・最小値

### 0020. 合計
- （お客さん）「単価が10万円の時、全ての荷主から荷物を受け付けたら、いくらもらえるんだろう。。。」
- （あなた）「妄想乙」

#### 回答
```sql
select
  sum(unit_rate * 100000)
from owners
;
```

#### 解説
- 合計はsum


## 結合
### 00021. 内部結合
- （お客さん）「オーダーを一覧で見せてくれ」
- （あなた）「ほい」
```
select
  *
from orders
;
```
- （お客さん）「なんだよこれ、荷主IDだけじゃ誰からのオーダーかわかんないよ。荷主名を出してくれよ」


#### 回答
```sql
select
  orders.*,
  owners.name as 荷主名
from orders
inner join owners
  on orders.owner_id = owners.id
;
```

#### 解説
- 二つのテーブルを紐付けるにはjoinを使う
- 紐付けるキーをonで指定


### 00022. 外部結合
- （お客さん）「オーダーとその明細を一覧で見たいんだけど。」
- （あなた）「ほい」
```
select
  *
from orders
inner join order_detail
  on orders.id = order_detail.order_id
;
```
- （お客さん）「さっきも言っただろマスタはIDじゃなくて名称を出して欲しいんだ。荷主と明細のサイズな」
- （あなた）「御意」

```
select
  *
from orders
inner join order_detail
  on orders.id = order_detail.order_id
inner join owners
  on orders.owner_id = owners.id
inner join size
  on order_detail.size_id = size.id
;
```
- （お客さん）「なんか列が多くて見難いな。各テーブルのversionは表示しなくていいや。後オーダーと明細のnoteも。それから、荷主とサイズは名前以外の情報は出さないでくれ」
- （あなた）「(select句書かないとな。テーブル名が長いのでasで別名つけるか)」

```sql
select
  o.id as オーダーID,
  o.date as オーダー日,
  ow.name as 荷主名,
  o.departure_post_code as 発地郵便番号,
  o.departure_address1 as 発地住所1,
  o.departure_address2 as 発地住所2,
  o.departure_address3 as 発地住所3,
  o.arrival_post_code as 着地郵便番号,
  o.arrival_address1 as 着地住所1,
  o.arrival_address2 as 着地住所2,
  o.arrival_address3 as 着地住所3,
  od.item_name as 荷物名,
  od.weight as 重量,
  s.name as サイズ
from orders as o
inner join order_detail as od
  on o.id = od.order_id
inner join owners as ow
  on o.owner_id = ow.id
inner join size as s
  on od.size_id = s.id
;
```

#### 解説
- 二つのテーブルを紐付けるにはjoinを使う
- 紐付けるキーをonで指定

