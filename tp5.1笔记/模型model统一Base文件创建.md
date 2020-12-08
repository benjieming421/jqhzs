model下创建Base文件

```php
namespace app\wechatsamll\model;


use think\Model;

class Base extends Model
{
    protected function nametest($value)
    {
        return $value.'这是修改后的';
    }
}
```



Testclass文件继承Base类

```php
namespace app\wechatsamll\model;


use think\Model;

class Testclass extends Base
{
    protected $table = 'test_class';

    public function getNameAttr($value)
    {
        return $this->nametest($value);
    }
}


```

结果 ![image-20201208205703371](https://i.imgur.com/J2lWYwn.png)

