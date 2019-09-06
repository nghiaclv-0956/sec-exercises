<p> Link Challange: http://ctfq.sweetduet.info:10080/~q6/ <br>
Do đề bài là SQLi, ta sẽ dùng SQLi để tìm Flag <br>
Dùng Burp Suite dựng proxy bắt request, sau đó Send To Repeater. <br> 
Với payload  <b>id =' or 1=1;--</b> ta login được, tuy nhiên ta chỉ nhận được nội dung là một đoạn code PHP và:
<pre>Congratulations!
It's too easy?
Don't worry.
The flag is admin's password.
</pre>
Cộng với việc tại trang chủ có nội dung  <b>First, login as "admin". </b> <br>

Như vậy ta phải SQLi để lấy mật khẩu, đồng thời cũng là Flag, với tên đăng nhập là <b>admin</b> <br>

Chuyển request sang Intruder, tại đây ta SQLi để dò độ dài mật khẩu. Khá giống với 1 challange trên Hacker101<br>

Payload từ 1 - 100, cho ra độ dài đúng là 21 <br>

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/length.png">

Tiếp tục sử dụng Intruder, với payload là Brute Force (default từ a-z, 0-9) để Blind SQL Injection, mất khoảng gần 1 phút và ra flag <b> flag kpwa4ji3uzk6trpk </b><br>

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/pass.png">

<br>
Position số 5 thử lại với các ký tự đặc biệt và ra kết quả là <b> _ </b>
Vậy flag là <b>flag_kpwa4ji3uzk6trpk</b>

Tuy nhiên submit với <b>id = admin</b> và <b>password = flag_kpwa4ji3uzk6trpk</b> không thành công <br>

=> Khả năng có phân biệt chữ thường và chữ IN HOA. Và khi thêm payloads chữ HOA vào Intruder(Brute Force) thì đúng là như vậy. Tuy nhiên với câu lệnh SQL trên thì không thể phân biệt LENGTH response khi payload là chữ thường/HOA. <br>

Tại đây có 2 hướng, một là tạo từ điển mật khẩu từ Flag đã biết (21 ký tự), 2 là Blind Injection theo cách khác.
Do đề bài là SQLi nên cứ SQLi cho dễ:

1. Thay đổi câu lệnh SQL
2. Thay đổi type attack trong Intruder sang Cluster bomb hoặc Pitchfork
3. Do đã có chính xác các ký trong password, ta thay đổi lại payloads để Brute Force (đỡ phải BF nhiều ký tự không cần thiết)

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/command.png">

<br>
Tấn công và nhận kết quả, ứng với mỗi position sẽ ra ký tự tương ứng. Nhìn không theo thứ tự lắm nhưng tấn công chỉ mất 1p, ráp lại cũng chỉ tốn khoảng 1 phút. Ước gì có 2 màn hình nhìn cho dễ @@ <br>

<i> <b> Tấn công với Cluster Bomb</b></i>
<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/result.png">


<i> <b> Hoặc tấn công với Pitchfork</b></i>

<img src="https://github.com/nghiaclv-0956/sec-exercises/blob/master/0x05/images/result2.png">

<br>
Position có Length 2361 là đúng ký tự, Có Length 769 thì chỉ việc thay bằng chữ HOA

<pre> FLAG_KpWa4ji3uZk6TrPK </pre>


</p>

