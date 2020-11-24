1. 在app目录下创建lib\exception文件夹

2. exception文件新建ExceptionHandler.php为开发者自定义类

```php
use app\admin\behavior\Log;  //日志
use Exception;				//php异常捕获
use think\exception\Handle;	//thinkphp异常捕获

class ExceptionHandler extends Handle
{
    private $code;
    private $msg;
    private $error;
    public function render(Exception $e)
    {
        if($e instanceof BaseException)  //判断$e捕获到的是否为自定义异常
        {
            $this->code = $e->code;
            $this->msg = $e->msg;
            $this->error = $e->error;
        }else{
            if(config('app_debug')){
                return parent::render($e);
            }
            $this->code = 500;
            $this->msg = '服务器内部错误，开发人员太水了';
            $this->errorCode = 999;
            $this->recordErrorLog($e);
        }
        $result = [
            'msg' => $this->msg,
            'error' => $this->error
        ];
        return json($result,$this->code);
    }

    private function recordErrorLog(Exception $e)
    {
        Log::record($e->getMessage(), 'error');
    }
}
```





3. 在config\app.php下的exception_handle改为`app\lib\exception\ExceptionHandler` 由开发者自定义类进行接管异常捕获

4. 创建lib\exception\BaseException.php

   ```php
   use think\Exception;
   
   
   class BaseException extends Exception
   {
       public $code = 400; //HTTP 状态码 200 400 302
   
       //错误的具体信息
       public $msg = '参数错误！';
   
       //自定义的错误码
       public $errorCode = 10000;
   
       /**
        * 构造函数，接收一个关联数组
        * @param array $params 关联数组只应包含code、msg和errorCode，且不应该是空值
        */
       public function __construct($params=[])
       { 
           if(!is_array($params)){
               return;
           }
           if(array_key_exists('code',$params)){
               $this->code = $params['code'];
           }
           if(array_key_exists('msg',$params)){
               $this->msg = $params['msg'];
           }
           if(array_key_exists('errorCode',$params)){
               $this->errorCode = $params['errorCode'];
           }
       }
   
   ```

5. 在`lib\exception\`下创建php文件自定义错误标明  文件名为`自定义文件名+Exception`

   ```php
   class CatogenException extends BaseException
   {
       public $code = 401;
   
       public $msg = '手动xx指出错误';
   
       public $error = 50000;
   }
   ```

   

   测试

   ```php
       public function excrender()
       {
           throw new CatogenException(); //手动抛出异常
       }
   	报出`{"msg":"手动xx指出错误","error":50000}`
   ```

   

