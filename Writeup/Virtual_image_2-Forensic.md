## 🕵️‍♂️ Challenge:

<img width="502" height="318" alt="image" src="https://github.com/user-attachments/assets/16929570-da37-4d44-a963-e4ddffc72dad" />

## 📝 Solution:
Từ bài Virtual image 1, ta đã có file `svhost.exe` là file mã độc, bây giờ mình sẽ bóc tách nó để lấy flag.  
Chạy thử file:  

<img width="983" height="738" alt="image" src="https://github.com/user-attachments/assets/811bba34-fa1f-4b67-925b-2bd63a867f31" />

Một chương trình sẽ hiện ra và bắt chúng ta nhập 1 chuỗi gì đó.  
Mình thử nhập sai thì nó biến màn hình máy tính của mình bị méo mó, xé hình và loạn màu tùm lum.  
Tiến hành kiểm tra file thì thấy file này được đóng gói bằng UPX:  

<img width="714" height="357" alt="image" src="https://github.com/user-attachments/assets/0cf3b621-266b-4b7e-87bf-6b909c00dc90" />

Giải nén nó:  

```bash
upx -d svhost.exe -o game.exe
```
<img width="953" height="258" alt="image" src="https://github.com/user-attachments/assets/022d1834-d703-4e2b-ad98-cadbdd5fee00" />

Mở file game.exe bằng IDA để reverse. Mình thử nhập sai chuỗi input thì nó báo:  

<img width="447" height="80" alt="image" src="https://github.com/user-attachments/assets/5dd91765-39ea-4673-8d19-10380a9a4446" />

Vậy nên trong IDA, mình shift + f12 để vào danh mục strings, sau đó ctrl + F để tìm cụm từ ***correct*** thì được:  

<img width="663" height="146" alt="image" src="https://github.com/user-attachments/assets/0748987b-0a43-42cd-80d9-02e8f2f83404" />

Nhấn đúp vào dòng đầu để tới vị trí của nó và quan sát các dòng xung quanh:  

<img width="1096" height="647" alt="image" src="https://github.com/user-attachments/assets/8a102fcc-60e2-45c1-985e-d79727987f45" />

Nhấn đúp vào hàm có chuỗi correct này, ta thấy được hàm check đúng sai của input:  

<img width="822" height="646" alt="image" src="https://github.com/user-attachments/assets/1baadd24-45b8-40a8-a7f8-001294cbc5cf" />

Xem code hàm check kia thì thấy nó lấy input rồi gọi sub_140001680(Paint) để kiểm tra.  

Xem thử hàm này thì thấy rõ nguyên lí sinh check:  

<img width="855" height="790" alt="image" src="https://github.com/user-attachments/assets/c8b61514-77b9-412e-9486-0bf486260a40" />

Hàm lấy từng byte trong bảng byte_1401436A0 XOR với ký tự v7 ±1 để sinh ra whatisthis.png.  

8 byte đầu tiên của file output buộc phải đúng PNG signature (89 50 4E 47 0D 0A 1A 0A).  

4 byte kế tiếp buộc là chiều dài chunk = 00 00 00 0D.  

4 byte kế tiếp buộc là **"IHDR"`.  

Dùng các công thức đảo ngược:  

Nếu i chẵn: pwd[i % L] = (H[i] ^ T[i]) - 1  

Nếu i lẻ : pwd[i % L] = (H[i] ^ T[i]) + 1  

Thế lần lượt 16 byte plaintext đầu (signature + length + “IHDR”) và các byte tiếp theo, ta có thể lấy lại được chuỗi:  

> NoCurrentThreats

Nhập thử thì nó báo đúng, và sinh ra 1 file png:  

<img width="502" height="301" alt="image" src="https://github.com/user-attachments/assets/e477aa05-dd8d-4e69-8dce-25daa8767881" />

<img width="1203" height="529" alt="image" src="https://github.com/user-attachments/assets/42f35f75-ba30-4ca0-a38f-314dd61f94a9" />

Nhìn có vẻ ngon ăn nhưng đây là flag giả, tiếp tục phân tích file này.  
Với những file ảnh PNG, mình quen tay dùng zsteg thì ra luôn flag:  

<img width="910" height="144" alt="image" src="https://github.com/user-attachments/assets/7a68ea84-0c27-4f0c-995b-f58307b29aba" />

Rõ ràng là nằm ở Forensic nhưng lại bắt RE -_-

>### 🎯 Flag: ***PTITCTF{L3ast_S1gn1f1cant_B1t_M4g1c_1n_3x3}***
