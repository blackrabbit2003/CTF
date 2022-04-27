# PIN
----
## Challenge
----
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/1.png)<br>
## Solution
Ta thấy được file rev1 là 1 dạng file thực thi `(ELF)`\
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/2.png) \
Cấp quyền execute cho file: `chmod +x rev1`<br>
Run file….\
![8.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/8.png)\
Oke giờ tôi sẽ sử dụng IDA để phân tích\
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/3.png)\
Oke, tôi đã thấy “**`Masukan PIN = `**”, đọc tiếp tôi thấy **`test eax, eax`** (tức là nếu **`eax`** bằng 0 nó sẽ nhảy đến **`loc_400623`**). Trước đó tôi thấy họ **`call cek`**,bạn có thể hình dung ra hàm này sẽ được gọi để so sánh giá trị và **`test eax, eax`** sẽ kiểm tra nó hợp lệ không, sau đó nó sẽ nhảy đến **`PIN benar!`** hoặc **`PIN salah!`**. Now, tôi sẽ đi vào kiểm tra nó.\
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/4.png)\
Oke, tôi thấy được nó sẽ bằng 0 khi **`rbp+var_4 != eax`**, mà trước đó **`eax`** được gán cho biến **`valid`**. Tôi sẽ kiểm tra tiếp **`valid`** và …. \
![5.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/5.png)\
Có vẻ như giá trị hex **`51615h`** là thứ mà đề bài đem ra so sánh.\
![6.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/6.png) \
Oke done\
![7.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/PIN/Capture/7.png) \
Vậy flag là : **`CTFlearn{333333}`**
