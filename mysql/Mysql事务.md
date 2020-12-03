mysql中的事物保证一组sql对表的修改或者删除操作要么全部功能，要么全部失败。

#### 事物开启方式

```mysql
begin;

update ...

delete ...

select ...

commit; // 成功就提交(commit)，失败就回滚(rollback);
```

需要注意的是