# CS331.M22.KHCL - THỊ GIÁC MÁY TÍNH NÂNG CAO - UIT


## MỤC LỤC
- [Giới thiệu](#giới-thiệu)
- [Giảng viên](#giảng-viên-hướng-dẫn)
- [Thành viên nhóm](#thành-viên-nhóm)
- [Đồ án](#đồ-án)
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

# ĐỒ ÁN
# KEY INFORMATION EXTRACTION FROM RECEIPT

## MỤC LỤC:

### Tại sao cần phải trích xuất thông tin hóa đơn ?

Khi một người có nhu cầu quản lý chi tiêu thì mô hình trích xuất thông tin này trở nên cần thiết vì nó trích xuất, lưu trữ và tính toán các khoảng tiền mà người đó đã chi tiêu trong 1 khoảng thời gian bằng cách đơn giản là chụp các hóa đơn.  

### Mô tả bộ dữ liệu



### Tiền xử lý dữ liệu 

Để làm sạch bộ dữ liệu hóa đơn, nhóm thực hiện alignments để đưa các object về cùng 1 hệ tọa độ. 

#### Alignments


### Trích xuất thông tin hóa đơn

Trong bài toán information extraction nhóm thực hiện sử dụng dạng dữ liệu là các hóa đơn (Receipt). Nhiệm vụ dặc ra là làm sao có thể phân loại các text box vào các trường thông tin tương ứng, bao gồm: Seller (Tên cửa hàng), Address (Địa chỉ cửa hàng), Timestamp (Ngày tạo hóa đơn), Total Cost (Tổng tiền)) và other (các trường mang thông tin không cần thiết).

Bài toán này được thực hiện khi ta thực hiện 2 bài toán con chính trước đó là: Scene Text Detection, Scene Text Recognition. Đầu ra của 2 bài toán con này được sử dụng để xây dựng các feature và đồ thị cho bài toán cuối cùng là Key Information Extraction. Đầu vào của mô hình là ảnh, đầu ra tương ứng với mỗi text box sẽ được phân loại thuộc 4 trường thông tin tương ứng.

Trước tiên nhóm thực hiện xác định vị trí hóa đơn có trong ảnh.

1. Detect hóa đơn trong ảnh

	#### YOLOv4

2. Text detection

Để chắc chắn cho một bộ dữ liệu sạch trước khi tiến hành text detection nhóm thực hiện thêm phần phân tích dữ liệu - EDA nhằm loại bỏ số hình bị lỗi, bị gắn sai label,.. còn sót bằng cách run 2 file: filter_training_data_by_rules.py và get_store_dict.py có trong repo github: https://github.com/ndcuong91/MC_OCR/tree/master/mc_ocr/EDA

- filter_training_data_by_rules.py: lấy những cái có những keyword sau:

		. TIMESTAMP: ['ngày', 'thời gian', 'giờ']
    
		. TOTAL COST: ['tổng tiền', 'cộng tiền hàng', 'tổng cộng', 'thanh toán', 'tại quầy']
    
--> Input là file cvs trong folder mc_ocr/data (có thể là file mcocr_train_df_filtered.csv) file csv bao gồm các value: img_id, anno_polygons, anno_texts, anno_labels, anno_num, anno_image_quality

Trong đó:

		. img_id: là tên của file image
    
		. anno_polygons: là toạ độ 5 cái bbox của từng img_id
    
		. anno_texts, anno_labels: là label của bbox cũng như chữ cái trong đó (SELLER|||ADDRESS|||TIMESTAMP|||TOTAL_COST|||TOTAL_COST)
    
		. anno_num, anno_image_quality: số lượng bbox detect dc và score của chúng.
    
==> file này giúp ta lọc ra những ảnh có label TIMPSTAMP và TOTAL COST trước
  
- get_store_dict.py: tương tự file py trên mà tại đây sẽ lọc 1 lần nữa những ảnh bên trên để lấy ra những ảnh có label SELLER và ADDRESS.

Sau đó nhóm mới thực hiện quá trình text detection sử dụng model Paddle.

	#### Paddle

3. Rotation corrector
4. Textline rotation
5. Text recognition

	#### VietOCR

4. Key information extraction

	#### KIE Model (PICK) 



