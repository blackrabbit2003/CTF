# RE_verseDIS 
-----
## Challenge
----
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/RE_verseDIS/Capture/1.png) 
## Solution
----
`Bài này tôi sẽ đi qua nhanh...` <br>
Sau khi kiểm tra thì nhận thấy đây là 1 tệp ELF **(tệp thực thi)** \
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/RE_verseDIS/Capture/2.png) \
Tôi sẽ cấp quyền thực thi cho file <br>
Sau khi run file, thì có vẻ như nó yêu cầu password sau đó đưa ra 1 đoạn rằng tôi đã đúng. \
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/RE_verseDIS/Capture/3.png) \
Tiếp tục, tôi sẽ sử dụng IDA để phân tách chương trình ra...Và nhận được: \
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/RE_verseDIS/Capture/4.png) \
TÔi đã thấy `Input password: `. Tiếp tục follow thì tôi tìm thêm được `Good job dude !!!` và `Wrong password` \
![5.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/RE_verseDIS/Capture/5.png) \
Hướng đi đúng của bài này có lẽ là mũi tên màu đỏ. Tôi sẽ follow nó.<br>
`test    eax, eax` 1 instruction khá là quen thuộc 😁. Mong rằng qua các bài trước bạn đã hiểu sơ sơ, và tôi sẽ cố gắng đi nhanh.<br>
Lướt lên trên 1 chút có vẻ bạn cũng để ý có 2 instruction `cmp     [rbp+var_8], 15h` và `cmp     [rbp+var_4], 15h`, nó đang cố so sánh với giá trị `0x15`. Chưa biết trước điều gì nên tôi sẽ xem mã giả của nó :v <br>
Và đây là đoạn code tôi cần để giải quyết bài toán 😋 
``` C
  for ( i = 0; i <= 21; ++i )
  {
    key2[i] = key[i];
    msg[i] = str[4 * i] ^ LOBYTE(key2[i]);
  }
  for ( j = 0; j <= 21; ++j )
  {
    if ( input[j] != msg[j] )
      stat = 0;
  }
  if ( stat )
    puts("Good job dude !!!");
  else
    puts("Wrong password");
```
Khá quen thuộc với phép xor, tôi đã tiếp xúc từ các bài trước 👍 <br>
Kiểm tra lần lượt các key vì ở đây nó đang cố gán `key` cho `key2` sau đó thực hiện phép toán rồi lấy ra flag 
>key =  'IdontKnowWhatsGoingOn'<br>
👉 Key2 = 'IdontKnowWhatsGoingOn'

Mong các bạn hiểu dòng này 😁 `msg[i] = str[4 * i] ^ LOBYTE(key2[i]);` \
Tôi sẽ kiểm tra bộ nhớ của `str` và từ đó có keygen cho bài này: 
``` Python
#!/usr/bin/env python3

flag = ''
key2 = 'IdontKnowWhatsGoingOn'
str  =  [0x8, 0x6, 0x2C, 0x3A,0x32, 0x30, 0x1C, 0x5C,0x1, 0x32, 0x1A, 0x12,
        0x45, 0x1D, 0x20, 0x30,0x0D, 0x1B, 0x3, 0x7C,0x13, 0x0F ]
for i in range(21):
    flag += chr(str[i] ^ ord(key2[i])) 
print(flag)
```
Giải thích tại sao tôi chỉ lấy `str[i]` thì khi tôi thử `str[i*4]`có vẻ như nó nằm ngoài phạm vi nên tôi đã lược bỏ đi 😙 \
Oke well done: 
Flag is **`AbCTF{r3vers1ng_dud3}`**