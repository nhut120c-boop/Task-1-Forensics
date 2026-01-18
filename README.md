Task Steganography

File type: 

File ảnh như : PNG, JPG, SVG...

File âm thanh như: WAV, AIFF..

File ném như: zip, docx, mp3...

Ví dụ về file ảnh

<img width="615" height="82" alt="image" src="https://github.com/user-attachments/assets/3e7d46f8-3d34-4d79-8658-cd55c3026a49" />

Ví dụ về file âm thanh

<img width="624" height="67" alt="image" src="https://github.com/user-attachments/assets/414c8602-7d08-4a18-a2db-f1bb5a3990ba" />

Ví dụ về file nén

<img width="1337" height="129" alt="image" src="https://github.com/user-attachments/assets/aa95f67d-073e-4bce-9271-c0510cdf0dc2" />

Cấu trúc của PNG:

Gồm magic byte là 8 byte đầu:

 89 50 4e 47 0d 0a 1a 0a
 
 Gồm các chunk, một chunk gồm 4 trường:

 Trường Length là 4 byte: cho biết độ dài của byte

 vidu: 00 00 00 0d

 Trường chunk type gồm 4 byte: cho biết tên của khối để nhận biết 

 ví dụ: 49 48 44 52 dịch ra là IHDR là tiêu đề ảnh

 Trường data gồm 13 byte sau chunk type

ví dụ:  00 00 03 84 00 00 02 58 08 02 00 00 00 đây là nội dung chứa chiều rộng, chiều cao...

Trường CRC gồm 4 byte cuối chunk: dùng để kiểm tra xem dữ liệu của khối có bị lỗi hay không

ví dụL: b5 a7 bf 8c, khi mở ảnh thì máy tính sẽ tính toán lại nếu b5 a7 bf 8c thì đúng
 
Cách nhận biết file type là:
Xài lệnh 
```
file
```
Hoặc nhìn vào các byte đầu của magic byte
```
 hexdump -C anh1.png | head
```
<img width="790" height="233" alt="image" src="https://github.com/user-attachments/assets/82572c8f-128a-4f11-9393-977244f790d3" />

Tìm hiểu về thư viện Pillow và Open CV

Open CV là 








