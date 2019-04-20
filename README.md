# PHP操作mysql实现增删改查

PHP笔记

> php链接数据库//init.php

```
<?php
   $conn = mysqli_connect('127.0.0.1','root','root','myapp');
   $sql = "SET NAMES UTF8";
   mysqli_query($conn,$sql);
?>
```



> 增加数据

```
<?php
require('init.php');
$raw_success = json_encode(array('code' => 1000, 'msg' => 'suc'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => 'err'));
@$uname = $_REQUEST['utel'] or die($raw_fail);
@$upassword = $_REQUEST['upassword'] or die($raw_fail);
@$utcode = $_REQUEST['utcode'] or die($raw_fail);
$utime = time();
//2:发送sql语句
$sql = "INSERT INTO myuser VALUES(null,'$uname','','$upassword','$utcode','$utime',0)";
$result = mysqli_query($conn,$sql);
 //3:判断返回结果


 if($result===true){
   echo $raw_success;

 }else{
   echo $raw_fail;
 }

?>
```
```
<form action="reg.php" method ="post">
	<input required type="text" name="uname">
	<input  type="text" name="upassword">
	<input type="" name="utcode">
	<input type="submit">
</form>
```

> 查询表中所有字段数据

```
<?php
require('init.php');
//2:发送sql语句
$sql = "select * from myuser limit 5";
$result = mysqli_query($conn,$sql);
 //3:判断返回结果

$rows= mysqli_fetch_all($result,MYSQLI_ASSOC);

if($rows){
	echo json_encode(array('code' => 1000, 'result' => $rows));
}else{
	echo json_encode(array('code' => 1001, 'msg' => 'err'));
}

?>
```

> 修改表中字段

```
<?php
require('init.php');
$raw_success = json_encode(array('code' => 1000, 'msg' => 'suc'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => 'err'));
@$uname =  $_REQUEST['uname'] or die($raw_fail);
@$upassword = $_REQUEST['upassword'] or die($raw_fail);
//2:发送sql语句
$sql = "update myuser  set upassword = '$upassword'   where uname = '$uname'";
$result = mysqli_query($conn,$sql);
 //3:判断返回结果

if($result===true){
	echo $raw_success;
}else{
	echo $raw_fail;
}

?>
```
```
<form action="gai.php">
	<input type="" name="uname">
	<input type="" name="upassword">
	<input type="submit">
</form>
```


> 删除表中数据//del.php?uid

```
<?php
require('init.php');
//2:发送sql语句
$raw_success = json_encode(array('code' => 1000, 'msg' => 'suc'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => 'err'));

@$uid = $_REQUEST['uid'] or die($raw_fail);
$sql = "delete  from myuser where uid = '$uid'";
$result = mysqli_query($conn,$sql);
 //3:判断返回结果

 if($result===true){
   echo $raw_success;

 }else{
   echo $raw_fail;
 }

?>
```

> php随机生成32位字符串转换成md5

```
<?php
function str_rand($length = 32, $char = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ') {
    if(!is_int($length) || $length < 0) {
        return false;
    }
    $string = '';
    for($i = $length; $i > 0; $i--) {
        $string .= $char[mt_rand(0, strlen($char) - 1)];
    }

    return $string;
}

echo json_encode(array('code' => 1000, 'msg' =>md5(str_rand())));
?> 
```

```
{
  "code": 1000,
  "msg": "4dfce12ee1aa2591c281810ef3f4021f"
}
```














