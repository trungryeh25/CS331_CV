# CS331.M22.KHCL - THỊ GIÁC MÁY TÍNH NÂNG CAO - UIT


## MỤC LỤC
- [Giới thiệu](#giới-thiệu)
- [Giảng viên](#giảng-viên-hướng-dẫn)
- [Thành viên nhóm](#thành-viên-nhóm)
## GIỚI THIỆU
- Tên môn học: Thị giác Máy tính Nâng cao
- Mã lớp: CS331.M22.KHCL

## GIẢNG VIÊN HƯỚNG DẪN
-  TS. 

## THÀNH VIÊN NHÓM

|STT| Họ Tên | MSSV| Email | Github |
|:-:|:------------------|:---------:|:--------:|:-----------:|
| 1 | Nguyễn Van A | 1952xxxx | 1952xxxx@gm.uit.edu.vn | ...  |
| 2 | Trương Van B | 1952**** | 1952****@gm.uit.edu.vn | .... |
| 3 | Nguyễn Van C | 1952x*x* | 1952x*x*@gm.uit.edu.vn | ... |

# ĐỒ ÁN: KEY INFORMATION EXTRACTION FROM RECEIPT.

## MỤC LỤC:

### Tại sao cần phải trích xuất thông tin hóa đơn ?

Khi một người có nhu cầu quản lý chi tiêu thì mô hình trích xuất thông tin này trở nên cần thiết vì nó trích xuất, lưu trữ và tính toán các khoảng tiền mà người đó đã chi tiêu trong 1 khoảng thời gian bằng cách đơn giản là chụp các hóa đơn.  

### Trích xuất thông tin hóa đơn

Trích xuất thông tin hóa đơn trong đồ án nhóm đề cập bao gồm việc sử dụng 1 mô hình (YoloV4/ detect & crop ra các trường dữ liệu (Seller, Address, Timestamp, Total Cost) có trong hóa đơn, sau đó dùng mô hình VietOCR để đọc các trường dữ liệu vừa có được.

#### Mô tả bộ dữ liệu



#### Detect các trường dữ liệu sử dụng YoloV4
1. Text detection

##### Sử dụng mô hình YOLOv4 cho detect

Object detection là một kĩ thuật trong thị giác máy tính được sử dụng để xác định tọa độ các đối tượng trên hình ảnh hoặc video. YOLOv4 là một máy dò tìm đối tượng single stage phổ biến trong detection và classification sử dụng CNNs. Mạng YOLOv4 bao gồm backbone mạng trích xuất feature và phát hiện các heads để xác định ví trí các đối tượng trong một hình ảnh.

YOLOv4 đạt được nhiều cải thiện về độ chính xác của mạng lưới tích chập (CNN) và tối ưu về tốc độ so với các phiên bản YOLO trước đó.

2. Text recognition


3. Key information extraction





