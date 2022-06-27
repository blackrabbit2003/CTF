# BROKEN
-----------
## Challenge
---
![2.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/2.png)<br>
## Solution
---
![1.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/1.png)<br>
Tệp định dạng bị sai nên em sẽ sử dụng trình chỉnh sửa hex. Ở đây em dùng `Hexed.it` <br>
![3.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/3.png)<br>
Việc đầu tiên em làm là sẽ sửa lại id Header : IHDR <br>
![4.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/4.png)<br>
`CRC` của file là `61 20 55 AA` <br>
![5.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/5.png)<br>
Còn 1 vấn đề duy nhất nữa là size của bức ảnh, thì sau 1 hồi tìm kiếm em tìm ra giải pháp code khắc phục nó:
```python
#!/usr/bin/env python3

from zlib import crc32

data = open("/home/phamtrungkien/Downloads/FPTUHacking/test2.png", 'rb').read()
index = 12

ihdr = bytearray(data[index:index+17])  # ihdr
width_index = 7  # width
height_index = 11  # height

for x in range(1, 2000):
    height = bytearray(x.to_bytes(2, 'big'))
    for y in range(1, 2000):
        width = bytearray(y.to_bytes(2, 'big'))
        for h in range(len(height)):
            ihdr[height_index - h] = height[-h - 1]
        for w in range(len(width)):
            ihdr[width_index - w] = width[-w - 1]
        if hex(crc32(ihdr)) == '0x612055aa':  # crc
            print("width: {} height: {}".format(width.hex(), height.hex()))
        for i in range(len(width)):
            ihdr[width_index - i] = bytearray(b'\x00')[0]
```
IHDR bắt đầu từ byte 12, nó là 17 byte trong chiều dài<br>
Và chạy chương trình trên ta có được kết quả của size ảnh ``width: 03f1=1009 height: 00da = 218``<br>
Sau khi chỉnh lại kích thước của file thì ta được kết quả như sau:
![7.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/7.png)<br>
![6.png](https://github.com/blackrabbit2003/CTF/blob/master/FPTU%20Hacking/Broken/6.png)<br>