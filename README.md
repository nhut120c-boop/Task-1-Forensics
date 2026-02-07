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

Open CV là 1 thư viện mã nguồn mở gồm hơn 2500 thuật toán cho máy tính có thể nhìn được vật thể,nó gồm 2 mảng chính là image processing và computer vision 

image processing dùng dể cắt ghép ảnh, đổi màu làm mờ.....

computer vision dùng để nhận diện người, đếm xe hay nhận diện trộm...

Các code py phổ biến với thư viện Open CV

lệnh cơ bản

đầu tiên khai báo
```
import cv2
```
cv2.imread(path, flag) đọc ảnh: flag=0: ảnh xám, flag=1: ảnh màu
```
ví dụ: xam = cv2.imread('anhtest.jpg', 0), thì file anhtest sẽ được đọc là màu xám
```
cv2.imshow(winname, mat) hiển thị ảnh
```
ví dụ: cv2.imshow('dis1', xam)
```
cv2.imwrite(tên ảnh, img, params) dùng chỉnh độ nén
```
cv2.imwrite('anhtest.jpg', xam, [cv2.IMWRITE_JPEG_QUALITY, 10])
```
cv2.cvtColor dùng để đổi màu sắc
```
anh = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```
ví dụ: cropped_img = img[100:300, 200:400]
```
cropped_img = img[y đầu:y cuối, x đầu:x cuối] dùng để cắt ảnh
```
cv2.putText(...): Viết chữ lên ảnh

```
cv2.putText(img, "zeroD", (50, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2)
```
cv2.resize(src, dsize) thay đổi kích thước ảnh
```
anh_nho = cv2.resize(img, (200, 200))
```
khai báo và đọc camera
```
cap = cv2.cam(0)

ret, frame = cap.read()
```
Khử nhiễu: cv2.GaussianBlur
```
ví dụ: blur = cv2.GaussianBlur(img, (5, 5), 0)
```
Tìm đường viền của vật thể: cv2.Canny
```
vien = cv2.Canny(img, 100, 200)
```
Lọc màu: cv2.inRange
```
mặt_nạ = cv2.inRange(ảnh_hsv, màu_thấp_nhất, màu_cao_nhất)

```
lật ảnh: cv2.flip
```
flip = cv2.flip(img, 1) # 1: lật ngang, 0: lật dọc, -1: cả hai
```

Giữ ảnh khi show không bị tắt
```
cv2.waitKey(0)
```
cv2.destroyAllWindows() để dọn ram
Còn các lệnh nữa... (em sẽ tìm hiểu từ từ ạ)


Công dụng trong for:

Nhận biết ảnh sửa chữa nhiều 

code ví dụ:
```
import cv2
import matplotlib.pyplot as plt

# Đọc ảnh
img = cv2.imread('evidence.jpg')

# Tính toán Histogram cho 3 kênh màu (BGR)
colors = ('b', 'g', 'r')
for i, col in enumerate(colors):
    hist = cv2.calcHist([img], [i], None, [256], [0, 256])
    plt.plot(hist, color=col)
    plt.xlim([0, 256])

plt.title('Biểu đồ Histogram - Kiểm tra mức độ phân bổ pixel')
plt.show()
```
Làm rõ ảnh: 
code ví dụ: 
```
import cv2
import numpy as np

img = cv2.imread('dark_evidence.jpg', 0) # Đọc ảnh hệ xám (grayscale)

# Cân bằng histogram để làm rõ chi tiết trong vùng tối
equ = cv2.equalizeHist(img)

# Hoặc dùng bộ lọc làm sắc nét (Kernel)
kernel = np.array([[-1,-1,-1], [-1,9,-1], [-1,-1,-1]])
sharpened = cv2.filter2D(img, -1, kernel)

cv2.imshow('Original', img)
cv2.imshow('Enhanced', equ)
cv2.waitKey(0)
```

Cắt ảnh, đổi màu ảnh:
code ví dụ: 
```
import cv2

img = cv2.imread('secret.png')
# 1. Cắt ảnh (Slicing mảng NumPy)
# Cú pháp: img[y_start:y_end, x_start:x_end]
crop_img = img[100:400, 200:500] 

# 2. Đổi màu ảnh (Color Space Conversion)
# Đổi sang Grayscale để dễ nhận diện biên (Edges)
gray_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# Đổi sang HSV để lọc các màu sắc cụ thể (ví dụ lọc màu đỏ)
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

# Lưu lại ảnh đã xử lý
cv2.imwrite('cropped_evidence.png', crop_img)

cv2.imshow('Cropped', crop_img)
cv2.imshow('Gray Scale', gray_img)
cv2.waitKey(0)
```
VÍ DỤ:
tạo 1 file png có nền đen và chữ KMA có màu GRB là 1,0,0:

<img width="1152" height="648" alt="anhtesst" src="https://github.com/user-attachments/assets/d09cd0c1-085a-4f6e-95ac-30ace9ba79f0" />

đưa ảnh vô máy ảo để bắt đầu lọc, đầu tiên tạo file soi.py

<img width="373" height="66" alt="image" src="https://github.com/user-attachments/assets/8b1a67dc-aa0d-4565-af97-ac0e3bb7915c" />

xong viết script để lọc màu:

<img width="533" height="137" alt="image" src="https://github.com/user-attachments/assets/a14a91ed-95ee-4415-9c87-da9d0bae9522" />

script để lọc bit cuối kênh red rồi phóng đại tương phản để làm hiện chữ ẩn, sau đó chạy py

<img width="405" height="64" alt="image" src="https://github.com/user-attachments/assets/c343c75f-c0eb-4fbc-8b6d-44775c362146" />

ta được

<img width="624" height="351" alt="image" src="https://github.com/user-attachments/assets/fe66045c-87f2-4d7d-9617-6f09d67406cf" />










