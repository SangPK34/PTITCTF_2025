## 🕵️‍♂️ Challenge:
### Một đoạn trao đổi bí ẩn giữa hai cá nhân đã bị ghi lại. Trong lúc trò chuyện, một tệp quan trọng dường như đã được chuyển qua lại. Hãy phân tích dữ liệu bạn thu thập được và tìm ra thứ mà họ không muốn để lộ.  

<img width="504" height="394" alt="image" src="https://github.com/user-attachments/assets/981d2ecb-e34d-496b-bd00-f47a3e2a447c" />

## 📝 Solution:
Tải file về và giải nén, ta được một file `capture.pcap`. Mở bằng wireshark để phân tích.  
Mình thấy có rất nhiều gói tin, và thử export dạng http xem sao:  
<img width="1864" height="974" alt="image" src="https://github.com/user-attachments/assets/e2bad241-5233-4ca8-8e95-e3567fa044ce" />
<img width="1136" height="737" alt="image" src="https://github.com/user-attachments/assets/55b57456-8eef-4c0b-bc01-c7613b933aa5" />

Vậy là ta xuất được các file cần từ các gói tin được truyền đi này.  
Ngoài các file khác thì mình chú ý đến file rar trước, nhưng nó đòi mật khẩu. Có vẻ phải phân tích các file khác để lấy mật khẩu.  
Sau khi phân tích chán chê 3 file ảnh và 1 file txt kia, thì mình thấy có vẻ chúng ko có giá trị gì, nhưng được cái là làm mất thời gian :D.  
Mình quyết định crack pass file rar bằng John.  
<img width="1076" height="697" alt="image" src="https://github.com/user-attachments/assets/c88ec183-cfcf-433c-bf4d-cde31246198b" />

Rất nhanh, pass là ***peanuts***  

Giải nén file rar, ta được rất nhiều file txt với tên từ `part_0000.txt` đến `part_0372.txt`.  

<img width="1230" height="703" alt="image" src="https://github.com/user-attachments/assets/5dee3d55-d994-41da-b9be-5aac81675acb" />

Đọc thử 1 số file thì có vẻ là các chuối base64 bị chèn vào đó là các kí tự rác như AAAA...  
Mình sẽ ghép tất cả các file txt lại thành 1 file hoàn chỉnh:  

```bash
printf "%s\n" part_*.txt | sort -V | xargs cat | tr -d '\r\n' > all.b64
```

File ghép được:  
<img width="1515" height="657" alt="image" src="https://github.com/user-attachments/assets/e80d0ec6-b175-434f-bffa-22b906c91cdc" />


Bây giờ loại mọi ký tự không thuộc bảng Base64:  
```bash
tr -cd 'A-Za-z0-9+/=' < all.b64 > all.clean.b64  
```

Ta được file mới sạch hơn:  
<img width="1462" height="619" alt="image" src="https://github.com/user-attachments/assets/411b0c4a-686c-4228-9f46-97f301f7e814" />

Decode thử đoạn đầu thì ta thấy có dấu hiệu của 1 file exe:  

<img width="1197" height="817" alt="image" src="https://github.com/user-attachments/assets/85c324af-8144-47c5-9248-cf00e59bf045" />

Vậy ta buid lại file exe từ file base64 sạch kia:  

```bash
base64 -d all.clean.b64 > output.exe  
```

Chạy thử file exe thì được file cờ caro:  

<img width="642" height="339" alt="image" src="https://github.com/user-attachments/assets/87dcac95-917a-4424-acef-2f597a707793" />

Nó bắt phải thắng 10000 lần mới đưa flag, và thua giữa chừng thì reset về 0, vậy nên ko thể chơi chay bài này được, khả năng phải reverse.  
Mình mở file bằng dnspy để tìm hàm chứa flag:  

<img width="1625" height="904" alt="image" src="https://github.com/user-attachments/assets/5329f7b1-edbf-44bf-815a-b710970cdc47" />

Mình tìm được hàm gọi flag:  

<img width="1608" height="907" alt="image" src="https://github.com/user-attachments/assets/306e6942-e3ea-4b6e-b3ca-171ef1f6954f" />

Flag được mã hóa dạng AES, sau khi đọc tất cả các hàm con, các byte được cung cấp, cùng với sự trợ giúp từ người thầy chát di pi ti, mình đã viết được 1 script python và đã giải được flag:  
```python
from Crypto.Cipher import AES

key_shard1 = [121,165,33,10,18,197,254,212,240,253,79,245,53,48,123,46,142,215,38,213,25,168,2,224,53,25,9,191,221,152,199,246]
key_shard2 = [63,201,64,109,80,176,151,184,148,152,61,183,76,120,26,71,224,179,22,230,56,137,35,193,20,56,40,158,252,185,230,215]
nonce = bytes([236,85,150,249,133,223,22,97,218,211,38,76])
tag = bytes([56,74,98,242,57,243,115,204,222,253,56,232,197,107,14,225])
ciphertext = bytes([174,48,7,100,207,26,27,150,166,144,90,153,225,176,222,113,164,197,167,77,133,132,235,43,43,115,86,82,85,184,28,28,219,201,31,202,70,19,137,96,159,89,137,51,168,115])

# XOR 2 key shards
secret_key = bytes([a ^ b for a, b in zip(key_shard1, key_shard2)])

cipher = AES.new(secret_key, AES.MODE_GCM, nonce=nonce)
plaintext = cipher.decrypt_and_verify(ciphertext, tag)
print("Flag:", plaintext.decode())

```
<img width="1078" height="78" alt="image" src="https://github.com/user-attachments/assets/84126fda-f091-4bf2-b8e6-0cc755185caa" />


Hehe

>### 🎯 Flag: ***PTITCTF{dotn3t_c4nn0t_mak4it_difficult_f0ry0u}***
