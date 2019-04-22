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

> 注册查重

```
<?php
require('init.php');
$raw_success = json_encode(array('code' => 1000, 'msg' => '用户注册成功'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => '用户注册失败'));
$raw_fail2 = json_encode(array('code' => 1010, 'msg' => '用户已存在'));

@$uname = $_REQUEST['uname'] or die($raw_fail);
@$upassword = $_REQUEST['upassword'] or die($raw_fail);
@$utcode = $_REQUEST['utcode'] or die($raw_fail);
$utime = time();

$sql = "select uname from myuser where uname = '$uname'";
$result = mysqli_query($conn,$sql);

$row = mysqli_fetch_row($result);

if(json_encode($row)==='null'){
	$sql = "INSERT INTO myuser VALUES(null,'$uname','','$upassword','$utcode','$utime',0)";
	$result = mysqli_query($conn,$sql);
	 //3:判断返回结果
	 if($result===true){
	   echo $raw_success;

	 }else{
	   echo $raw_fail;
	 }
}else{
	echo $raw_fail2;
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



> 查询每日，每周，每月数据

```
<?php
require('init.php');
//2:发送sql语句
ini_set('date.timezone','Asia/Shanghai');
$raw_success = json_encode(array('code' => 1000, 'msg' => 'suc'));
$raw_fail = json_encode(array('code' => 1001, 'msg' => 'err'));
$start=date('Y-m-d 00:00:00');
$end=date('Y-m-d H:i:s');
//$sql = "select * from myuser WHERE `utime` >= unix_timestamp('$start') AND `utime` <= unix_timestamp('$end')";//查询当天数据：

//$sql = "select count(*) from myuser WHERE `utime` >= unix_timestamp('$start') AND `utime` <= unix_timestamp('$end')";//查询当天数量：
//$sql = "select FROM_UNIXTIME(utime,'%Y-%m-%d') as days,count(*) count from myuser group by days"; //统计每日总数，为空不补0
//$sql="SELECT * FROM `myuser` WHERE YEARWEEK( FROM_UNIXTIME( `utime`,'%Y-%m-%d %H:%i:%s') ,1) = YEARWEEK( now( ),1 )";//查询本周数据：


$starty=date('Y-m-01 00:00:00');
$endy=date('Y-m-d H:i:s');
$sql="SELECT * FROM `myuser` WHERE `utime` >= unix_timestamp('”.$starty.”') AND `utime` <= unix_timestamp('$endy')";//查询本月数据：



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


> 获取时间

```
//1、php获取今日开始时间戳和结束时间戳
$beginToday=mktime(0,0,0,date('m'),date('d'),date('Y'));
$endToday=mktime(0,0,0,date('m'),date('d')+1,date('Y'))-1;	 
echo$beginToday.'---'.$endToday;
echo'

//2、php获取昨日起始时间戳和结束时间戳
$beginYesterday=mktime(0,0,0,date('m'),date('d')-1,date('Y'));
$endYesterday=mktime(0,0,0,date('m'),date('d'),date('Y'))-1;
echo$beginYesterday.'---'.$endYesterday;
echo'

//3、php获取上周起始时间戳和结束时间戳
$beginLastweek=mktime(0,0,0,date('m'),date('d')-date('w')+1-7,date('Y'));
$endLastweek=mktime(23,59,59,date('m'),date('d')-date('w')+7-7,date('Y'));
echo$beginLastweek.'---'.$endLastweek;
echo'
//4、php获取本月起始时间戳和结束时间戳
$beginThismonth=mktime(0,0,0,date('m'),1,date('Y'));
$endThismonth=mktime(23,59,59,date('m'),date('t'),date('Y'));
echo$beginThismonth.'---'.$endThismonth;
echo'

```










