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