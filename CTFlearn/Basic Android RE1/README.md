# Basic Android RE 1
`*A simple APK, reverse engineer the logic, recreate the flag, and submit!*` <br>
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/1.png)
Tôi sẽ sử dụng apktool. Nếu chưa cài đặt thì gõ: `sudo apt-get install apktool`
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/2.png)<br>
Kiểm tra folder ta thấy: <br>
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/3.png)
Sau 1 hồi tìm thủ công ta thấy được 1 folder : **secondapp**<br>
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/4.png)
Có vẻ như hàm main được viết trong **MainActivity.smali**<br>
![5.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/5.png)
Đọc 1 lúc, tôi nhận ra const-strings chứ chuỗi kí tự của hàm Main()<br>
![6.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/6.png)
Tiếp tục:<br>
![7.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/7.png)
Ta thử flag: **“CTFlearn{b74dec4f39d35b6a2e6c48e637c8aedb_is_not_secure!}”**<br>
`*Oh!! You can solve this twice. Hmm..*`<br>
![8.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/8.png)
Ta dịch ngược mã MD5 được text: **Spring2019** <br>
![9.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Basic%20Android%20RE1/capture/9.png) 
Now, flag is **CTFlearn{Spring2019_is_not_secure!}**
