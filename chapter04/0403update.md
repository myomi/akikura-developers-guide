# 4-3. write - UPDATE文

### 2001. 全件更新（単項目）
- （お客さん）「全荷主の単価レートを1.50にしろ！」
- （あなた）「金の亡者め」

#### 回答
```sql
update owners 
set 
  unit_rate = 1.50
;
```

#### 解説
- update文で更新

### 2001. 全件更新（複数項目）
- （お客さん）「全荷主の単価レートを今の２倍にしろ！ついでに解約日を今日に設定しろ」
- （あなた）「...病気だ」

#### 回答
```sql
update owners 
set 
  unit_rate = unit_rate * 2,
  cancellation_date = current_date
;
```

#### 解説
- 複数項目