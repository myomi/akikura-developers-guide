# 4-1. read - SELECT文

## 検索の基本
### 0001. 全件検索
（お客さん）「荷主のデータを全て見たいんだが」

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
（お客さん）「さっきの荷主全データ、単価レートの低い順に並び替えて欲しいんだけど」

#### 回答
```sql
select
  *
from owners
order by
  unit_rate asc
;
```