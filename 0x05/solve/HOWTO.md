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
Như vậy ta phải SQLi để lấy mật khẩu, tên đăng nhập là admin.
Chuyển request sang Intruder, tại đây ta SQLi để dò độ dài mật khẩu. <br>


</p>

