## 🕵️‍♂️ Challenge:

<img width="504" height="658" alt="image" src="https://github.com/user-attachments/assets/c32c4299-541a-468b-90c4-77605e2f4670" />

## 📝 Solution:
Tải file về và giải nén, ta được một file data có tên `Hacker.ad1`  
Dựa vào đuôi .ad1 thì đây là file ổ đĩa được tạo bởi phần mềm FTK imager.  
Dùng chính phần mềm này để mở nó lên, sau khi tìm mọi ngóc ngách thì mình thấy 1 file khả nghi có tên `svhost.exe` gần giống với tên 1 tiến trình hệ thống là `svchost.exe`, vậy đây được coi là kẻ giả mạo cần tìm.  

<img width="1200" height="593" alt="image" src="https://github.com/user-attachments/assets/4b2daa1c-d364-41aa-a572-74b33c3f07e1" />

Đáp thử file này lên Virustotal thì báo đỏ lòm:  

<img width="1273" height="786" alt="image" src="https://github.com/user-attachments/assets/50267fe5-497e-4fda-a484-8d937d72d718" />

Khả năng cao ThreatLabel là  

> Trojan

Vậy ta xác đinh tiếp các phần còn lại của flag:  

<img width="728" height="341" alt="image" src="https://github.com/user-attachments/assets/e24a0be3-abb1-422c-b053-ab9b1551e4ba" />


Path:  

> C:\Users\Hacker\AppData\Roaming\Microsoft\Windows\svhost.exe

<img width="1103" height="373" alt="image" src="https://github.com/user-attachments/assets/045510ef-ba7d-4d8d-9276-e8d8044a16ec" />

MD5:  

> 0f266b60a2bd1818d752a3b6f3098b1c

<img width="446" height="117" alt="image" src="https://github.com/user-attachments/assets/ee1f5521-94ea-4a5c-8990-45f69a5e07ca" />

TimeUTC:  

> 2025-07-23T02:56:24Z

Ghép các mảnh flag lại, ta được flag hoàn chỉnh!

>### 🎯 Flag: ***PTITCTF{C:\Users\Hacker\AppData\Roaming\Microsoft\Windows\svhost.exe_0f266b60a2bd1818d752a3b6f3098b1c_2025-07-23T02:56:24Z_trojan}***
