

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
LINK: http://ksnctf.sweetduet.info/problem/31
Gồm 1 trang có 2 Submit Button đơn giản và source code của nó.

