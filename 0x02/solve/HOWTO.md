

----------------------------------------------------------------------------------------------------------------------------------
<p> <strong> BÀI 2: </strong><br>
Link: http://ksnctf.sweetduet.info/problem/32 <br>
Lỗ hổng của function: Strcasecmp() - Dùng để so sánh 2 string <br>
"Strcasecmp() function sẽ <b>return 0</b> nếu có bất kỳ parameters nào là một <b>array</b>" <br>
Dùng Postman, Request với KEY là <b>password[]</b> và VALUE bất kỳ, ta có được FLAG </p>
<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x02/images/02-1.png"> <br>
-----------------------------------------------------------------------------------------------------------------------------------
<br>

<strong> BÀI 4: </strong><br>

Link: http://ksnctf.sweetduet.info/problem/35 <br>
Bao gồm 1 trang đăng nhập tại: http://ctfq.sweetduet.info:10080/~q35/auth.php <br>
Và Source Code: <br>
<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x02/images/code2.png">

<br>
Từ Source Code, đặc biệt là đoạn:
<pre>
$s = $db->prepare('SELECT * FROM user WHERE id=? AND password=?');
</pre>
<br>
Ta có thể thấy trang login không dính SQL Injection. Login thử cũng không có Cookie, Session. <br>

Bỏ qua XSS vì đây là tấn công Sever Side (Simple Auth 2), code cũng khá cẩn thận với hàm htmlspecialchars() <br>

Cũng không include() hay require() bất cứ file nào để có thể dính File Inclusion <br>

Ngoài ra đây là Source Code bên  dưới: <br>
<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x02/images/code.png">

Vậy Password cũng chính là mật khẩu. Với kinh nghiệm từ những bài trước thì thường Flag sẽ có 21 ký tự, song song với đó là chưa biết username. Vậy phương án Brute Force cũng gần như không thể.<br>

Bài này chỉ có 20point, về lý không thể khó hơn bài 2 50point được.<br>
Và vấn đề được gỡ khi để ý đoạn code: <br>
<pre> $db = new PDO('sqlite:database.db');</pre><br>

Vậy là ngoài file <b>auth.php</b> sẽ còn có file <b>database.db</b> <br>
Ta sửa đường dẫn URL thành: <pre> ctfq.sweetduet.info:10080/~q35/database.db </pre> <br>

Browser tự động tải về 1 file <b>database.db</b>, mở file lên ta có được username và password chính là Flag cần tìm.

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x02/images/resultb4.png">

-----------------------------------------------------------------------------------------------------------------------------------<br>
<b>BÀI 3</b>
LINK: http://ksnctf.sweetduet.info/problem/31 <br>
Gồm 1 trang có 2 Submit Button đơn giản và source code của nó.
```php
<?php
$salt = 'FLAG_????????????????';
$shipname = array(
    'Nagato',
    'Mutsu',
    'Kongo',
    'Hiei',
    'Haruna',
    'Kirishima',
    'Fuso',
    'Yamashiro',
    'Ise',
    'Hyuga',
    "Yamato [Congratulations! The flag is $salt. ??????????????????????????????????????.]"
);

//  Check signature and read
if (isset($_COOKIE['ship']) and
    isset($_COOKIE['signature']) and
    hash('sha512', $salt.$_COOKIE['ship']) === $_COOKIE['signature'])
    $ship = explode(',', $_COOKIE['ship']);
else
    $ship = array();

if (isset($_POST['submit']))
{
    //  Gacha
    if ($_POST['submit'] === 'Gacha')
    {
        //  Yamato is ultra rare
        $ship[] = mt_rand(0, count($shipname)-2);

        $s = implode(',', $ship);
        $sign = hash('sha512', $salt.$s);

        setcookie('ship', $s);
        setcookie('signature', $sign);
    }

    //  Clear
    if ($_POST['submit'] === 'Clear')
    {
        setcookie('ship', '', 0);
        setcookie('signature', '', 0);
    }

    header("Location: {$_SERVER['REQUEST_URI']}");
    exit();
}
?>
```
<br>
Ta thấy Flag được lưu ở param $salt. <br>
Nhìn vào đoạn code trên và xem Cookie của web, ta thấy rằng mỗi khi Submit, param  <b>$ship</b> sẽ được random một value ngẫu nhiên từ 0 => 9. <br>
Những param này sẽ được chuyển thành một string, ngăn cách bởi dấu "," nhờ vào hàm implode(), và được lưu vào param  <b>$s</b>. <br>
Param  <b>$sign</b> sẽ chứa value khi hashing bằng SHA512 với data nằm ở param <b>$salt</b> và salt nằm ở param  <b>$s</b> <br>
Khi submit form, ta sẽ có 2 cookie là <b>ship = $s</b>  và <b>signature = $sign</b>
<br>
<br>
<br>
Và đoạn code phía dưới <br>


```php
<?php

for ($i=0; $i<count($ship); $i++)
    echo "<li>{$shipname[$ship[$i]]}</li>\n";

?>
```

<br>
Nhìn vào đoạn source code trên, ta thấy không thể nào in ra được position số 10 (chứa <b>$salt</b> = Flag), do value <b>$ship</b> có max value =9 <br>

Vậy bây giờ phải làm sao đấy để ta có được value hashed với <b>$s = 10</b>, và nó dẫn dắt tới một loại tấn công tên "hash length extension attack" và tool được biết đến nhiều nhất là <b>HashPump</b><br>

Cài đặt và chạy HashPump:<br>


<br>
Trong đó:<br><pre>
<b>Input Signature</b>: Chính là hashed cookie được web trả về, mặc định đúng vì được web hashed và gán cho Client.<br>
<b>Input data</b>: Value dùng làm salt, ta biết được nhờ value của ship.<br>
<b>Input Key Length</b>: Độ dài của value được băm, ta không biết được nội dung nhưng biết độ dài là 21 (chính là Flag).<br>
<b>Input Data to Add</b>: Là dữ liệu muốn thêm vào để được một dữ liệu mới hợp lệ. Do mặc định mỗi khi Submit thì các value được hàm implode() nối thêm một dấu phẩy phía trước, nên value thêm vào ta điền <b>,10</b> <br></pre>

Rất tiếc khi chạy thì không thành công, và vấn đề nằm ở đoạn: <br>

<pre><artical>3\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xb0,10
 </artical> </pre><br>
