```
date("Y-m-d H:i:s",time())  时间
```

# 查询

### 查询全部

```php
Db::table("tp_user")->select(); Db::table(表名)->select();

select()
```

### 查询一条

```php
find()
```

### 显示sql语句

```php
getLastSql()  方法只能获取上一次执行的SQL记录。
    
使用方式：User::getLastSql();
```

### 抛出

```php
findOrfail()  抛出异常
    
findOrEmpty() 抛出空数组

selectOrfail()  抛出异常
    
selectOrEmpty() 抛出空数组
```

### 查询单个字段值

```php
value('字段名')

使用方式：\db("user")->where("id",1)->value("username")
```

### 查询某一列

```php
column('字段名','可选id') column('字段名',"可定id字段的值作为索引 返回所有数据")
    
使用方法：\db:("user")->where('status',1)->column('username',id)
```

### 模糊查询

```php
常用：
//单个查询，查询有3的passwd值的集合  
Db::name('user')->whereLike('passwd','%3%')->select();

//多个查询，查询有think和php的值集合
Db::name('user')->where('name','like',['%think%','%php%'],'OR')->select();
```

### 区间查询

```php
常用：
//查询id2到22之间的数据
Db::name('user')->whereBetween('id',[2,22])->select();

//和上面效果相同
Db::name('user')->where('id','between',[2,22])->select();
```

### 选择查询

```php
常用：
////查询id2和22的数据
Db::name('user')->whereIn('id', '1,22')->select();

//和上面效果相同
Db::name('user')->where('id','in',[1,5,8])->select();
```

### 自定义查询

```php
常用：
Db::name('user')->whereExp('id', 自定义sql语句)->select();
例：
Db::name('user')->whereExp('id', 'IN (2,22)')->select();
```

## 查询字段

`field`方法



## 查询 助手函数

```php
\db("user")

使用方法：\db("user")->selectOrFail();
```

# 链式操作

### 关联数组

```php
// 传入数组作为查询条件
Db::table('think_user')->where([
	'name'	=>	'thinkphp',
    'status'=>	1
])->select(); 
```

用于方便查询

用于IN查询操作   `in常用于where表达式中，其作用是查询某个范围内的数据。`

### 索引数组

```php
// 传入数组作为查询条件
Db::table('think_user')->where([
	['name','=','thinkphp'],
    ['status','>',1]
])->select(); 
```

用于条件查询

### 字符串

```php
Db::table('think_user')->where('type=1 AND status=1')->select(); 
```

### 字符串 使用变量

```php
方法一：
    Db::table('think_user')
    ->where("id=:id and username=:name")
    ->bind(['id' => [1, \PDO::PARAM_INT] , 'name' => 'thinkphp'])
    ->select();

方法二：
    Db::table('think_user')
    ->whereRaw("id=:id and username=:name",
               ['id' => [1, \PDO::PARAM_INT] , 'name' => 'thinkphp'])
    ->select();

区别方法一多了bind，方法二链式变成了whereRaw
```

# 插入

### 常用

```php
$arr = [
         'username' => '辉夜',
         'passwd'    => '665',
         'sex'      =>   '女'
        ];
Db::name('user')->data($arr)->insert();
```



### 获取插入成功ID

```php
insertGetId()
    
使用：
    return Db::getLastInsID(); 获取第一条数据id
    Db::name('user')->getLastInsID($data);  获取第一条数据id
```

# 更新

### 常用

update 方法返回影响数据的条数，没修改任何数据返回 0

```php
//修改id为8的数据   1
$data = [
           'username' => '辉夜大小姐',
           'id'       => '8'
        ];

Db::name('user')->update($data);

//修改id为8的数据   2
$data = [
           'username' => '辉夜大小姐',
           'id'       => '8'
        ];
Db::name('user')->where('id',8)->update($data);
```

### 函数操作

```php
raw方法  *可进行减法，加法，字符串字母大写小写等

Db::name('user')
    ->where('id', 1)
    ->update([
        'name'		=>	Db::raw('UPPER(name)'),
        'score'		=>	Db::raw('score-3'),
        'read_time'	=>	Db::raw('read_time+1')
    ]);

```

### 修改一条

setField 方法返回影响数据的条数，没修改任何数据字段返回 0

```php
Db::name('user')->where()->setField(字段, 值);

例:
  Db::name('user')->where('id',5)->setField('username','马可师傅');
```

# 删除

### 常用

```php
Db::name('user')->where('表达式')->delete();

删除主键极简方法
Db::name('user')->delete(40);
Db::name('user')->delete([20,30,23]);

例：
Db::name('user')->where('id>=15 and id<=20')->delete();
```



### 删除表所有数据

```php
Db::name('user')->delete(true);
```

