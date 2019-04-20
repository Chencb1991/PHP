# PHP
PHP笔记

> 增加数据

```
<?php
require('init.php');
@$uname = $_REQUEST['utel'] or die('{msg:err}');
@$upassword = $_REQUEST['upassword'] or die('{msg:err}');
@$utcode = $_REQUEST['utcode'] or die('{msg:err}');
$utime = time();
//2:发送sql语句
$sql = "INSERT INTO myuser VALUES(null,'$uname','','$upassword','$utcode','$utime',0)";
$result = mysqli_query($conn,$sql);
 //3:判断返回结果
$raw_success = json_encode(array('code' => 1000, 'msg' => 'suc'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => 'err'));

 if($result===true){
   echo $raw_success;

 }else{
   echo $raw_fail;
 }

?>
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




