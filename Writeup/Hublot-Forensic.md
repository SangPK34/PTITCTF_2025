## 🕵️‍♂️ Challenge:
### Just a Song 🎵 (Steganography)

Bạn nhận được một file nhạc MP3. Hãy lắng nghe…. Mọi thứ bạn cần đều nằm trong chính file — không cần tìm kiếm bên ngoài.  

<img width="625" height="566" alt="image" src="https://github.com/user-attachments/assets/13009757-b3cc-4d54-94a8-a32c9aef06e0" />
  
## 📝 Solution:
Đề cho 1 file có tên **`challenge.mp3`**, kiểm tra thì có vẻ đây là 1 file mp3 hợp lệ:  

<img width="1352" height="93" alt="image" src="https://github.com/user-attachments/assets/5fff8158-46db-4b79-b9f5-c75f3553373a" />
<br>
<br>
Mình nghe thì âm thanh cũng ko có gì lạ, mình có thử mở bằng Audacity để quan sát hình dạng sóng xem có flag ẩn ko thì cũng ko thấy gì.  
Thử dùng lệnh binwalk để giải nén xem có file ẩn bên trong ko:  
<br>
<br>
<img width="1482" height="303" alt="image" src="https://github.com/user-attachments/assets/d614367b-89b9-4845-b9ea-71e7e68eb9bf" />
<br>
<br>
Thì đây, 1 file **`flag.txt`** nẳm trong 1 file zip tên **0xC059** đã được nén vào trong file mp3 của chúng ta, bây giờ hãy thử giải nén nó nhé!  
Oh, nó đòi mật khẩu. Vậy chúng ta lại quay lại file mp3 ban đầu để tìm thêm dữ kiện về mật khẩu này.  
Thử xem metadata của file:  

<img width="805" height="636" alt="image" src="https://github.com/user-attachments/assets/48a67951-5344-44f4-9cf6-3873f70a97c5" />
<br>
<br>
Xuất hiện gợi ý về 1 chuỗi base64, hãy thử decode nó:  

<img width="776" height="73" alt="image" src="https://github.com/user-attachments/assets/4b443849-fb61-4d54-bd4d-e26562faa13c" />
<br>
<br>
Vậy pass là `Hehe@@`, giải nén file zip kia, ta được file `flag.txt` có nội dung chứa flag.  

<img width="237" height="24" alt="image" src="https://github.com/user-attachments/assets/b634a7c1-0ca4-489c-9fbf-fa69db75101a" />
<br>
<br>
 Hehe :))

>### 🎯 Flag: ***PTITCTF{Warm_Up_so_Crazy}***
