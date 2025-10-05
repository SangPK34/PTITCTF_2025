## 🕵️‍♂️ Challenge:
### Một bản nhạc, một ngọn nến, một trò chơi. Thắp lên rồi xem bạn tìm thấy gì, và tôi sẽ để lại cho bạn 1 mẩu giấy, có gắn liền với các ngọn nến để bạn dễ dàng tìm kiếm hơn nhé. 🕯️🎼  

<img width="646" height="484" alt="image" src="https://github.com/user-attachments/assets/65dbdbf5-ebce-4684-b0b9-fa36d0e9d918" />

## 📝 Solution:
Tải 2 file về và phân tích, mình nghe thử file mp3 thì bị ngay cái giọng meme mèo cười vào mặt:))  
Thử kiểm tra thì file Pepper là dạng data, do nội dung ko đọc được nên mình để sang 1 bên, còn file mp3 kia cũng hợp lệ, ko có gì bất thường.  
Mình thử binwalk file mp3 thì được 1 đống dữ liệu nén bên trong:  

<img width="824" height="689" alt="image" src="https://github.com/user-attachments/assets/92d75c37-45e3-4223-abfe-e9e6a1750ff7" />

Bây giờ giải nén để lấy chúng, ta được 30 file png khác nhau:
<img width="1369" height="194" alt="image" src="https://github.com/user-attachments/assets/5759e2a0-f3cb-43e6-8729-41d3f146ab01" />

Xem thử có vẻ đây là các mảnh QR được cắt nhỏ ra, tổng cộng 30 mảnh.
<img width="1120" height="623" alt="image" src="https://github.com/user-attachments/assets/524d045a-4514-4491-8fbe-8b702b1fd976" />

Có thể, file Pepper kia sẽ có script để ghép chúng lại, nhưng thay vì phân tích file Pepper, mình đưa đống ảnh lên Canva ghép cho nhanh :))  
Và thành quả là:  
<img width="1265" height="630" alt="image" src="https://github.com/user-attachments/assets/cf6e6358-a263-43a5-a917-fb9f0a97276e" />

Đem đi decode thì ta được 1 link:  
<img width="944" height="759" alt="image" src="https://github.com/user-attachments/assets/df26fe1a-40aa-459b-a68a-bb5995e86078" />  

Truy cập link X đó, ta tới 1 bài viết:  

<img width="778" height="737" alt="image" src="https://github.com/user-attachments/assets/19777b30-d443-47cf-be1d-c77f747f2dee" />  

Dưới bình luận bài viết, có bình luận là 1 chuỗi base64:  
 
 <img width="743" height="134" alt="image" src="https://github.com/user-attachments/assets/b2e34edb-5f06-4113-bea6-0a2db2883855" />

Đem đi decode thử, ta được 1 flag:  

<img width="773" height="533" alt="image" src="https://github.com/user-attachments/assets/bbed6c44-574d-41d6-87d3-179b3dacc229" />

Thật ko may, khi nộp thì đây là 1 flag giả, vậy tiếp tục tìm kiếm lại trong bài đăng trên, mình thấy dưới comment của cái comment bas464 đó có 1 đoạn rất quen:  

<img width="742" height="137" alt="image" src="https://github.com/user-attachments/assets/be71c99a-8b32-4842-912d-8f65f78bf378" />

Rõ ràng, đây là định dạng của 1 đường link, nhưng có vẻ là Rot13, đem đi giải mã ta được đường link chuẩn:

> https://github.com/AFatc4t/notthetruth/blob/main/candlegame.exe

Đây là 1 file exe, tải về chạy thử thì là 1 game chứng khoán:

<img width="1284" height="878" alt="image" src="https://github.com/user-attachments/assets/789d1e0f-b4a6-498c-bee7-064e8ac5363d" />

Mục tiêu phải đạt được 100000000$, nếu chơi chay thì chắc phải đến năm sau mới lên được, còn mình thử để thua thì màn hình máy mình bị vô hiệu hóa ko thao tác được, kèm theo đó là kênh discord của tác giả :))  
Mình nghĩ ra 1 hướng đó là thay đổi giá trị tiền hiện có, bởi đây là game offline đơn giản nên có khá nhiều tool, trong đó có tool quen thuộc với ae game thủ là ***cheat engine***.  
Chạy game và cheat engine song song, vào cheat engine chọn tiến trình của game làm mục tiêu.  

<img width="1764" height="661" alt="image" src="https://github.com/user-attachments/assets/8b3d4694-24f5-469c-94e8-e4027dcbee73" />

Bây giờ ở cheat engine tìm kiếm giá trị tiền đang có là 1000$, ta lọc được kha khá:  

<img width="1249" height="665" alt="image" src="https://github.com/user-attachments/assets/c39da640-fc4f-4cfb-b77e-959ce798ba2e" />

Bây giờ thay đổi giá trị tiền hiện có ở game, sau đó lọc tiếp để thu hẹp đối tượng, từ đó sẽ tìm ra được mục tiêu cần sửa là giá trị tiền.  

<img width="1765" height="639" alt="image" src="https://github.com/user-attachments/assets/b59c82e1-87fa-4623-96c0-32eb17ae246e" />

Đây rồi, là cái này, giờ đổi thành giá trị bất kì lớn hơn 100000000$ là được, sau đó ở tab game, chọn mua hoặc bán để cập nhật giá trị tiền, ta sẽ thấy flag nhảy ra:

<img width="1751" height="738" alt="image" src="https://github.com/user-attachments/assets/9b8b2436-dd42-4ec6-ad9c-2386e2b46c60" />

BÙMMMM


>### 🎯 Flag: ***PTITCTF{PTIT_Futures_is_a_crypt0currency_futures_trading_platf0rm_and_sh0rt_BTC_set_up_n0w!!!}***
