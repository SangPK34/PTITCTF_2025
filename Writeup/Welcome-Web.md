## 🕵️‍♂️ Challenge:
### Hidden in message ! Check Discord.  

## 📝 Solution:

 Vì bài này ko nằm trong danh mục web nên ban đầu mình ko nghĩ đến hướng giải web, mình thử vào kênh discord CTF của cuộc thi, tìm thử các đoạn chat của kênh xem có gì đáng ngờ ko thì thấy:  
 <br>
 <img width="1451" height="945" alt="image" src="https://github.com/user-attachments/assets/b0c63e44-894b-4a71-be29-984a744c32c8" />
<br>
<br>
Mình thử flag này thì ko đúng. Sau 1 thời gian mày mò suy nghĩ gợi ý từ đề bài, mình mới chú ý đến cụm từ "Message".  
Mình nhấn F12 để mở Devtools, chọn mục network, nhấn f5 để reload, lọc thử cụm từ message thì có ngay:  

<img width="1771" height="633" alt="image" src="https://github.com/user-attachments/assets/b1b06521-5631-4341-9a66-bd87d4ab394e" />
<br>
<br>
Nhấn vào request có tên ***messages?limit=50***, chọn tab respone, tìm trong đó và bắt được flag ẩn giấu cuối dòng:  

<img width="1248" height="763" alt="image" src="https://github.com/user-attachments/assets/fd3f8f67-8190-4da5-a3c2-64bcf7bb0760" />
<br>
<br>


>### 🎯 Flag: ***PTITCTF{We1c0m3_T0_PT!TCTF2@25}***
