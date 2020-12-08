https://github.com/firebase/php-jwt

https://blog.csdn.net/cjs5202001/article/details/80228937



```php
$key = 'sjYdklasQr';
$payload = array(
  'iat' => time(),      // jwt的签发时间
  'nbf' => time(),     //定义在什么时间之前，某个时间点后才能访问
  'exp' => time()+3600,  //过期时间1h
  'userinfos' => [
      'uid' => $info['id']
  ]
);

$jwt = JWT::encode($payload,$key);
return json(['code'=>1,'msg'=>'okk','token'=>$jwt],200);
```





前端获取token放到缓存，每次请求api在header带上token

后台获取header里的token进行揭秘验证

