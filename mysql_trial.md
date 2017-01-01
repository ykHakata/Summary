# NAME

mysql_trial - mysql 使い方簡単まとめ

# USAGE

最低限の使い方

```sql
-- データーベースのカラムを削除
-- ALTER TABLE table DROP [COLUMN] column
-- 例: staff テーブルの name カラムを削除
ALTER TABLE staff DROP COLUMN name;

-- 短く
ALTER TABLE staff DROP name;

-- name, birthday 二つを削除
ALTER TABLE staff
    DROP COLUMN name,
    DROP COLUMN birthday;
```

# SEE ALSO

関連項目

- <http://dev.mysql.com/doc/refman/5.6/ja/> MySQL 5.6 リファレンスマニュアル (日本語)
