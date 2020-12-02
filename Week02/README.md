## 作业 
我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

## 思路 
dao层类似底层基础库不应该返回errors.Wrap 而返回 root err, 只在业务代码中Wrap

```go
func GetUser(name string) (*User, error) {

	sqlStr := "select name from user where name=?"
	var u user
	err := db.QueryRow(sqlStr, name).Scan(&u.name)
	
	switch {
        case err == sql.ErrNoRows: 
             return nil,errors.New("not found user")
        case err != nil:
           	return nil, err
        case err == nil:
            return &User{name:name},nil
    }
}
```