## 🕵️‍♂️ Challenge:
### Neco finally became a Summoner, but now she accidentally summons herself! And one of her summons has hidden malware inside the system, can you find the malware?  
PS: restore the "Neco" snapshot to access the challenge.  

<img width="636" height="533" alt="image" src="https://github.com/user-attachments/assets/886fef89-13f1-47ff-8174-f8d4356cb1cd" />

## 📝 Solution:
Tải file về và giải nén, ta được một folder khá nặng, chứa các file của 1 máy ảo window 7.  

 <img width="1128" height="859" alt="image" src="https://github.com/user-attachments/assets/7add8775-31e5-451c-ac01-229f0afdeb10" />

Mình mở thử bằng VMware thì thấy khá dài và khó :D có rất nhiều file png lớn nhỏ, và nhiều ngóc ngách cần thăm dò.  

Nên dựa vào đề bài, mình chú ý đến file snapshot này.  

<img width="795" height="84" alt="image" src="https://github.com/user-attachments/assets/531930e5-ec5d-4d26-8df3-00c213aa7039" />


grep thử chuỗi PTITCTF trong file này xem sao:  

<img width="1434" height="88" alt="image" src="https://github.com/user-attachments/assets/fae73cfe-2b20-48e3-81b5-f8a8a6c75661" />

Ơ :)) Có vẻ tác giả cũng ko ngờ tới điều này...

>### 🎯 Flag: ***PTITCTF{W1P3R!!!_S3nd_h3Lp_P1e4Sss!!!}***
