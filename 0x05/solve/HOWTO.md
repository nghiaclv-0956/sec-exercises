<p> Link Challange: http://ctfq.sweetduet.info:10080/~q6/ <br>
Do đề bài là SQLi, ta dễ thấy sẽ dùng SQLi để tìm Flag <br>
Dùng Burp Suite dựng proxy bắt request, sau đó Send To Repeater. <br> 
Với payload trong id là <b>' or 1=1;--</b> ta login được, tuy nhiên ta chỉ nhận được nội dung là một đoạn code PHP và:
<pre>Congratulations!
It's too easy?
Don't worry.
The flag is admin's password.
</pre>
Cộng với việc tại trang chủ có nội dung  <b>First, login as "admin". </b> <br>

Như vậy ta phải SQLi để lấy mật khẩu, với tên đăng nhập là <b>admin</b> <br>

Chuyển request sang Intruder, tại đây ta SQLi để dò độ dài mật khẩu. Khá giống với 1 challange trên Hacker101<br>

Payload từ 1 - 100, cho ra độ dài đúng là 21 <br>

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/length.png">

Tiếp tục sử dụng Intruder, với payload là Brute Force để Blind SQL Injection <br>

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/pass.png">
</p>

