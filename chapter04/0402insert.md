# 4-2. write - INSERT文

### 1001. １件登録
- （お客さん）「新しい荷主が増えるので登録してくれないか」
- （あなた）「そろそろ荷主登録画面作りましょうよ」

#### データ登録依頼書

| name | unit_rate | contract_date | cancellation_date |
| -- | -- | -- | -- |
| 夙川家具 | 1.95 | 2016-03-01 | 未設定 |

※idは自動採番、versionは0で設定すること。

#### 回答
```sql
insert into owners(
  name, 
  unit_rate, 
  contract_date, 
  cancellation_date, 
  version
) values (
  '夙川家具', 
  1.95, 
  to_date('2016-03-01', 'YYYY-MM-DD', 
  null,
  0
);
```

#### 解説
- insert文で新規データが登録できる
- カラム名を明示的に指定しなくても書けるが、その場合は全てのカラムに登録値を指定する必要がある。

