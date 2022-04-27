# Reykjavik
----
## Challenge
----
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/1.png)
## Solution 
---
Unzip file của đề bài : `unzip Reykjavik.zip` \
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/2.png) \
Oke… \
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/3.png) \
Có vẻ như định dạng password đúng là `CTFlearn{flag}`. Tôi sẽ sử dụng IDA để phân tích nó ra. <br>
Đọc qua mã 1 lượt thì như bạn thấy ở đây \
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/4.png) \
Tôi tìm được `"Congratulations, you found the flag!!: "` và `"Sorry Dude, '%s' is not the flag :-(\n"` <br>
Để ý 1 chút, tôi thấy được hàm `cmp     edi, 1`, tức là so sánh thanh ghi `edi` với giá trị 1, nếu nó bằng thì nó sẽ nhảy đến `loc_11B5`, còn không thì nó sẽ đi theo mũi tên đỏ **(Bài này tôi sẽ cố gắng hướng nó theo mũi tên đỏ)**. \ 
![5.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/5.png) \
Tiếp tục phân tích, thấy instruction `sbb al,0` và `test al, al` ( đại loại nó sẽ là kiểm tra giá trị `al` ) \
![6.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/6.png) \ 
Thanh ghi `rax` khá là phức tạp, đặc biệt bạn sẽ nhận thấy có 1 instructions `xor eax,eax` và sau đó là `call __printf_chk` \
![7.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/7.png) \
Oke, giờ tôi sẽ nhấn F5 để xem mã giả của nó \
```C
int __cdecl main(int argc, const char **argv, const char **envp)
{
    const char *v3; // rbp
    int v4; // r12d
    __int64 v6[3]; // [rsp+0h] [rbp-38h] BYREF
    char v7; // [rsp+18h] [rbp-20h]
    char v8; // [rsp+19h] [rbp-1Fh]
    char v9; // [rsp+1Ah] [rbp-1Eh]
    char v10; // [rsp+1Bh] [rbp-1Dh]

    if ( argc == 1 )
    {
        v4 = 1;
        puts("Usage: Reykjavik CTFlearn{flag}");
    }
    else
    {
        v3 = argv[1];
        __printf_chk(1LL, "Welcome to the CTFlearn Reversing Challenge Reykjavik v2: %s\n", v3);
        puts("Compile Options: ${CMAKE_CXX_FLAGS} -O0 -fno-stack-protector -mno-sse\n");
        if ( !strcmp(v3, "CTFlearn{Is_This_A_False_Flag?}") )
        {
            v4 = 2;
            __printf_chk(1LL, "You found the false flag Dude!:'%s'\n\n", "CTFlearn{Is_This_A_False_Flag?}");
            }
            else
            {
                v10 = 0;
                v6[0] = data ^ 0xABABABABABABABABLL;
                v6[2] = qword_4020 ^ 0xABABABABABABABABLL;
                v6[1] = qword_4018 ^ 0xABABABABABABABABLL;
                v7 = byte_4028 ^ 0xAB;
                v8 = byte_4029 ^ 0xAB;
                v9 = byte_402A ^ 0xAB;
                v4 = strcmp((const char *)v6, v3);
                if ( v4 )
                {
                    v4 = 4;
                    __printf_chk(1LL, "Sorry Dude, '%s' is not the flag :-(\n\n", v3);
                }
                else
                {
                    __printf_chk(1LL, "Congratulations, you found the flag!!: '%s'\n\n", (const char *)v6);
                }
            }
        }
        return v4;
}
```
Có vẻ đây chính là nơi ta cần giải quyết :) \
![8.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/8.png) \
Tôi đoán nó đang cố XOR với 0xAB, tôi sẽ xem bộ nhớ của các biến 
![9.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/9.png) \
Tôi có code Keygen cho đề bài này: <br>
```python
#!/usr/bin/env python3
data        = [0xC5,0xD9,0xCA,0xCE,0xC7,0xED,0xFF,0xE8]
qword_4018  = [0xDD,0x9B,0xE7,0xF4,0xCE,0xD2,0xEE,0xD0]
qword_4020  = [0xC5,0xCA,0xC7,0xCE,0xC8,0xE2,0xF4,0xCE]
byte_4028   = 0xCF
byte_4029   = 0xF4 
byte_402A   = 0xD6 
xor_var = 0xAB

flag =''
flag1=''
flag2=''
flag3=''
for i in range(len(data)):
    flag1 += chr(data[i] ^ xor_var)
print(flag1[::-1])
for i in range(len(qword_4018)):
    flag2 += chr(qword_4018[i] ^ xor_var)
print(flag2[::-1])
for i in range(len(qword_4020)):
    flag3 += chr(qword_4020[i] ^ xor_var)
print(flag3[::-1])
flag += flag1[::-1] + flag2[::-1] + flag3[::-1] + chr(byte_4028^ xor_var) + chr(byte_4029 ^ xor_var) + chr(byte_402A ^ xor_var)
print(flag)
``` 
Oke well done!!  
![11.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Reykjavik/Capture/11.png) <br>

Now, Flag is **`CTFlearn{Eye_L0ve_Iceland_}`**
