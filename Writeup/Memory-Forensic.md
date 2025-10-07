🕵️‍♂️ Challenge:
### Trong môi trường thực hành của tôi đang bị nhiễm 1 số mã độc, người dùng đã gỡ bỏ nó nhưng vẫn còn dấu vết,có vẻ như họ cố tình để lại cho tôi 1 số thông điệp. Hãy tìm kiếm và giải mã.  
<img width="505" height="390" alt="image" src="https://github.com/user-attachments/assets/3c2f39b1-132f-442a-9030-9bba982610f0" />  

## 📝 Solution:
Tải file về và giải nén, ta được một file có tên `memory.raw`, kiểm tra bằng lệnh `file` thì nó báo là data, mình mở thử bằng notepad thì thấy có vẻ đây là 1 file chứa 1 hệ điều hành.  


<img width="1531" height="769" alt="image" src="https://github.com/user-attachments/assets/2a79c504-f2c9-495e-b6c8-a597fe955406" />
 
Tiến hành cài đặt Volatility3 để phân tích file:
```bash
pip install volatility3
```
Sau khi cài xong, ta chạy lệnh sau để kiểm tra thông tin hệ điều hành trong file:  
```bash
vol -f memory.raw windows.info
```

<img width="1457" height="641" alt="image" src="https://github.com/user-attachments/assets/232cd573-dcbc-4d2c-ab38-62c411bb5e40" />

Ta chạy lệnh sau để hiển thị các tiến trình đang chạy trong máy:  
```bash
vol -f memory.raw windows.pslist
```

<img width="913" height="826" alt="image" src="https://github.com/user-attachments/assets/ebcb9f83-1bf5-4c31-a62c-ad41279e35b8" />

Mình thấy có 2 tiến trình đáng chú ý: KeePass.exe (PID 2820) và notepad++.exe (PID 3612).  Notepad++ thường được dùng để lưu tạm nên có thể chứa chuỗi quan trọng; KeePass lưu DB mật khẩu → khả năng chuỗi mật khẩu / key nằm trong RAM.  
Tiến hành dump vùng nhớ tiến trình Notepad++ (lấy pid.3612.dmp)

```bash
mkdir -p out/npp_mem
vol -f memory.raw -o out/npp_mem windows.memmap --pid 3612 --dump
```

Vậy là đã có file dump vùng nhớ tiến trình Notepad++: `out/npp_mem/pid.3612.dmp`, bước tiếp là quét chuỗi trong file này để tìm key/iv/cipher.  

```bash
strings out/npp_mem/pid.3612.dmp | grep -iE 'PTIT|AES|KEY|IV|PassDB|base64|CBC' | sed -n '1,120p'
```

<img width="761" height="452" alt="image" src="https://github.com/user-attachments/assets/6dd3e2fb-0e16-4ea5-8fb1-73e2ba832fdb" />

Mình tìm được 3 mảnh: PTIT_CTF2025_KEY, InitializationVe và một ciphertext base64.  
Bây giờ phải giải mã AES (base64 → AES-CBC → plaintext)

```bash
python3 - <<'PY'
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import base64

KEY = b'PTIT_CTF2025_KEY'
IV  = b'InitializationVe'
B64 = 'rNxBkug3ri07khz2rKqQY+bv6GyhHZD/gbM4y2lUAUDENzGNDYeu1eNCWl9cTkyo'

pt = AES.new(KEY, AES.MODE_CBC, IV).decrypt(base64.b64decode(B64))
try:
    print("Decrypted message:", unpad(pt,16).decode())
except:
    print("Decrypted message (raw):", pt.decode(errors='ignore'))
PY
```
<img width="731" height="315" alt="image" src="https://github.com/user-attachments/assets/abd47f6f-f61a-4d81-b97a-0e47b0f963fb" />

Như vậy từ đoạn giải mã được, ta đã có mật khẩu dùng để mở KeePass DB:  

> NoCurrentThreatsInVirus&Protection

Tìm tiếp file trong RAM xem có file nào khả nghi không.  
```bash
vol -f memory.raw windows.filescan | egrep -i '\.kdbx|\.docx|\.dat|.txt|database|keepass'
```
<img width="1463" height="896" alt="image" src="https://github.com/user-attachments/assets/cc0b9a0d-c0f0-4814-96ce-35e6635b21f8" />

Mình chú ý đến các file đáng nghi như:

> 0x830473042a10  \Users\REM\Desktop\Hacker\database.kdbx   
> 0x830473de33a0  \Users\REM\Desktop\Hacker\real.txt   
> 0x830473f2d080  \Users\REM\Documents\real.txt   

Dump những file đó để kiểm tra nội dung:  

```bash
mkdir -p out/files
vol -f memory.raw -o out/files windows.dumpfiles --virtaddr 0x830473042a10
vol -f memory.raw -o out/files windows.dumpfiles --virtaddr 0x830473de33a0
vol -f memory.raw -o out/files windows.dumpfiles --virtaddr 0x830473f2d080

ls -lh out/files
for f in out/files/*; do echo "==> $f"; file "$f"; done
```
<img width="1125" height="574" alt="image" src="https://github.com/user-attachments/assets/01aa3261-34bb-4a6f-85cf-c952f930cd7e" />

Như vậy, Vol3 đã dump ra 3 file trong out/files, trong đó:  
.kdbx.dat là KeePass DB, còn 2 file .real.txt.dat là CDFV2 Encrypted → Có vẻ là Office file  được mã hoá.  
Bây giờ, mở KeePass DB bằng mật khẩu đã giải:  

```bash
python3 - <<'PY'
from pykeepass import PyKeePass
db = "out/files/file.0x830473042a10.0x830473b18c20.DataSectionObject.database.kdbx.dat"
kp = PyKeePass(db, password="NoCurrentThreatsInVirus&Protection")
for e in kp.entries:
    print(e.title, "|", e.username, "|", e.password, "|", (e.notes or "")[:120])
PY
```
<img width="800" height="218" alt="image" src="https://github.com/user-attachments/assets/b863d8b1-f203-4b46-a009-f8744bd88093" />

Trong DB có entry realdocx với password NowY0uC4nF1ndM3! — chính là mật khẩu để mở file Office.  
Thử giải mã file real.txt.dat bằng mật khẩu từ KeePass

```bash
python3 - <<'PY'
import msoffcrypto
inp = "out/files/file.0x830473f2d080.0x8304728a1a70.DataSectionObject.real.txt.dat"
pwd = "NowY0uC4nF1ndM3!"
out = "decrypted.docx"
with open(inp,"rb") as f:
    of = msoffcrypto.OfficeFile(f)
    of.load_key(password=pwd)
    with open(out,"wb") as g:
        of.decrypt(g)
print("OK ->", out)
PY
```
<img width="771" height="263" alt="image" src="https://github.com/user-attachments/assets/f1c2a1b4-2288-40f7-be0d-7f0d1947e06d" />

Mở thử bằng word, thì ngon luôn!  

<img width="1216" height="875" alt="image" src="https://github.com/user-attachments/assets/0696221a-75b5-44c3-b206-eaf1e72e9aa8" />

Doneee!


>### 🎯 Flag: ***PTITCTF{M3m0ry_Dumppppppppp!}***
