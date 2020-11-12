# 创建：

### 模块下创建model目录

### 创建数据库表名一样的文件名 如tp_user  文件名为User.php

### User类继承Model类

```php
<?php

namespace app\sample\model;

use think\Model;

class User extends Model
{

}
```

# 使用

### 引入创建的model类

```php
use app\sample\model\User;   app\模块名\model\文件名

    public function getModelData()
    {
        $data = User::select();  //使用查询
        return json($data);
    }
```

