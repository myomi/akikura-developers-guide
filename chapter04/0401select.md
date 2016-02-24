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
- （お客さん）「さっきの荷主全データ、単価レートの低い順に並び替えて欲しいんだけど」

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
並び替えはorder by句

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
- （お客さん）「全顧客の単価レートを２倍で表示してみてくれ」
- （あなた）「悪い事考えてるでしょ」


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
- 文字列連結の演算子は ||
- 文字列リテラルはシングルクオートで囲む