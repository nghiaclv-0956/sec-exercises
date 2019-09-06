

----------------------------------------------------------------------------------------------------------------------------------
<p> <strong> BÀI 2: </strong><br>
Lỗ hổng của function: Strcasecmp() - Dùng để so sánh 2 string <br>
"Strcasecmp() function sẽ <b>return 0</b> nếu có bất kỳ parameters nào là một <b>array</b>" <br>
Dùng Postman, Request với KEY là <b>password[]</b> và VALUE bất kỳ, ta có được FLAG </p>
<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x02/images/02-1.png"> <br>
-----------------------------------------------------------------------------------------------------------------------------------
<br>

<strong> BÀI 4: </strong><br>

Link: http://ksnctf.sweetduet.info/problem/35 <br>
Bao gồm 1 trang đăng nhập tại: http://ctfq.sweetduet.info:10080/~q35/auth.php <br>
Và Source Code:
<article>
<pre>
<?php

function h($s)
{
    return htmlspecialchars($s, ENT_QUOTES, 'UTF-8');
}

if (!isset($_POST['id']) or !is_string($_POST['id']))
    $_POST['id'] = '';
if (!isset($_POST['password']) or !is_string($_POST['password']))
    $_POST['password'] = '';

$try = false;
$ok = false;

if ($_POST['id']!=='' or $_POST['password']!=='')
{
    $try = true;
    $db = new PDO('sqlite:database.db');
    $s = $db->prepare('SELECT * FROM user WHERE id=? AND password=?');
    $s->execute(array($_POST['id'], $_POST['password']));
    $ok = $s->fetch() !== false;
}

?>
</pre>
</article>


Từ Source Code, đặc biệt là đoạn:
<pre>
$s = $db->prepare('SELECT * FROM user WHERE id=? AND password=?');
</pre>
<br>
Ta có thể thấy trang login không dính SQL Injection. Login thử cũng không có Cookie, Session. <br>

Bỏ qua XSS vì đề bài là <b>"Simple Auth 2"</b> và đây là tấn công Sever Side, code cũng khá cẩn thận với hàm htmlspecialchars() <br>

Cũng không include() hay require() bất cứ file nào để có thể dính File Inclusion <br>

Ngoài ra đây là Source Code PHP bên  dưới:
<artical>
<pre>

<?php if($try and $ok) { ?>
        <div class="alert alert-success">
          Congraturation!<br>
          The flag is <?php echo h($_POST['password']); ?>
        </div>

</pre>
</artical>

Vậy Password cũng chính là mật khẩu. Với kinh nghiệm từ những bài trước thì thường Flag sẽ có 21 ký tự, song song với đó là chưa biết username. Vậy phương án Brute Force cũng gần như không thể.



