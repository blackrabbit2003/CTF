# Service 0x1
----
## Challenge
![1.png](https://raw.githubusercontent.com/blackrabbit2003/CTF/master/Pwnable_warmup/1.png) <br>
 - Đầu tiên, nhìn dòng 3, cho phép v4 chứa **32** kí tự và dòng 26, v4 được scanf đến **48** giá trị --> BOF
- Tiếp theo nhìn đến việc khởi tạo giá trị của bài toán: <br>
![3.png](https://raw.githubusercontent.com/blackrabbit2003/CTF/master/Pwnable_warmup/3.png)<br>
- Khoảng giá trị của v4 nằm dưới v5 và dưới v8 (v4 < v5 < v8) như này: <br>
| +3Ch | <-- v8 <br>
|      |  <br>
| +30h |  <-- v5 <br>
|      |   <br>
| +10h |  <-- v4 <br> 
|      |   <br>
|  rsp |  <br>
|      |   <br>

Khi ta ghi **48** byte lên v4 thì những byte thừa sẽ được chèn hết vào mảng giá trị của v5, kế đó ta chỉ cần nhập 1 kí tự thì kí tự đó sẽ được lưu trữ trong mảng của v8. Có một vài phép màu nào đó khi ta thử với các kí tự lạ không phải số thì bài toán đã được giải quyết :)) <br>
![4.png](https://raw.githubusercontent.com/blackrabbit2003/CTF/master/Pwnable_warmup/4.png)<br>