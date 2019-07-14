## ストアドファンクション(SSMS)
```sql
CREATE FUNTION fc_hoge
(@iArg1 nvarchar(100), @iArg2 nvarchar(100))
RETURNS nvarchar(10)
AS
  BEGIN
    DECLEAR @val nvarchar(10)
  SET
    @val = (
      SELECT
        hoge
      FROM
        table
      WHERE
        column1 = @iArg1
        AND
        column2 = @iArg2
    )
```