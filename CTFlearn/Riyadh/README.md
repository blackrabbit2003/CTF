# Riyadh 
---
## Challenge
----
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Riyadh/Capture/1.png) 
## Solution
----
Oke, mức easy nên tôi sẽ lướt qua....<br>
Giải nén tệp zip và nhận được các file \
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Riyadh/Capture/2.png) \
Tiếp tục với series sử dụng IDA🥲 <br>
Tôi nghĩ mình nên chú ý vào những instruction này \
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Riyadh/Capture/3.png) \
Tôi nhìn thấy `All Done!`, khá gì và này nọ🙁
Có vẻ nhìn đây ta thấy kết hợp với việc phân tích mã giả thì `Msg5` là hàm chính <br>
Và chú ý rằng tôi đang cố hướng nó theo mũi tên đỏ \
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/CTFlearn/Riyadh/Capture/4.png) \
Xem `Msg5` có gì....
``` C
char __fastcall Msg5(char *a1)
{
  unsigned __int64 v2; // rdi
  __int64 i; // rax

  *(_QWORD *)a1 = 0LL;
  v2 = (unsigned __int64)(a1 + 8);
  *(_QWORD *)(v2 + 240) = 0LL;
  memset((void *)(v2 & 0xFFFFFFFFFFFFFFF8LL), 0, 8 * (((unsigned int)a1 - (v2 & 0xFFFFFFF8) + 256) >> 3));
  if ( (unsigned __int64)(a1 + 7 - (char *)&xormask) <= 0xE || (unsigned __int64)(a1 + 7 - (char *)&fmsg) <= 0xE )
  {
    for ( i = 0LL; i != 31; ++i )
      a1[i] = *((_BYTE *)&xormask + i) ^ *((_BYTE *)&fmsg + i);
  }
  else
  {
    *(_QWORD *)a1 = xormask ^ fmsg;                 //Yếu tố giải quyết toán này ở đây 
    *((_QWORD *)a1 + 1) = qword_4168 ^ qword_4078; 
    *((_QWORD *)a1 + 2) = qword_4170 ^ qword_4080;
    *((_DWORD *)a1 + 6) = qword_4178 ^ dword_4088;
    *((_WORD *)a1 + 14) = WORD2(qword_4178) ^ word_408C;
    LOBYTE(i) = BYTE6(qword_4178) ^ byte_408E;
    a1[30] = BYTE6(qword_4178) ^ byte_408E;
  }
  return i;
}
```
Tôi đoán key nó sẽ tạo thành từ việc các `qword` và `dword` xor với nhau. TÔi có keygen của bài này: 
``` Python
#!/usr/bin/env python3

xormask = [0x4B, 0x7A, 0xB1, 0x7E, 0x11, 0x80, 0x46, 0x9A]    
fmsg = [0x25, 0x08, 0xD0, 0x1B, 0x7D, 0xC6, 0x12, 0xD9]       
qword_4168 = [0xEA, 0x14, 0x1B, 0xB0, 0xAC, 0xF3, 0xBE, 0x01] 
qword_4170 = [0x5B, 0xF2, 0x48, 0xF3, 0x9C, 0x3F, 0xAD, 0x27] 
qword_4178 = [0xE7, 0x01, 0x8E, 0xA8, 0x42, 0x63, 0x7E, 0xFC] 
qword_4078 = [0xB5, 0x7F, 0x7A, 0xDD, 0xDF, 0x92, 0xF3, 0x7A] 
qword_4080 = [0x28, 0x81, 0x2D, 0x81, 0xE8, 0x4D, 0xC2, 0x61] 
dword_4088 = [0x74, 0x5B, 0x4F, 0xA3]                        
word_408C = [0xF3, 0x9D]                                     

flag =''  
flag1 =''
for i in range(len(xormask)-1, -1, -1):    
    flag += chr(xormask[i] ^ fmsg[i])  
print(flag)
for i in range(len(qword_4080) -1, -1, -1):    
    flag += chr(qword_4168[i] ^ qword_4078[i])  
print(flag)
for i in range(len(qword_4080) -1, -1, -1):    
    flag += chr(qword_4170[i] ^ qword_4080[i])  
print(flag)
for i in range(len(dword_4088) -1, -1, -1):   
    flag += chr(dword_4088[i] ^ qword_4178[i + 4]) 
print(flag)

flag += chr(qword_4178[3] ^ word_408C[1]) + chr(qword_4178[2] ^ word_408C[0]) 
print(flag)
```
Oke done <br>
Flag is: **`CTFlearn{Masmak_Fortress_1865}`**