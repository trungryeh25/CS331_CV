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
| 3 | Nguyễn Van C | 1952x*x* | 1952x*xx*@gm.uit.edu.vn | ... |

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
    
	-> Input là file cvs trong folder mc_ocr/data (có thể là file mcocr_train_df_filtered.csv) file csv bao gồm các value: img_id, anno_polygons, anno_texts, anno_labels, anno_num, anno_image_quality

	Trong đó:

		. img_id: là tên của file image
    
		. anno_polygons: là toạ độ 5 cái bbox của từng img_id
    
		. anno_texts, anno_labels: là label của bbox cũng như chữ cái trong đó (SELLER|||ADDRESS|||TIMESTAMP|||TOTAL_COST|||TOTAL_COST)
    
		. anno_num, anno_image_quality: số lượng bbox detect dc và score của chúng.
    
	-> file này giúp ta lọc ra những ảnh có label TIMESTAMP và TOTAL COST trước
  
	- get_store_dict.py: tương tự file py trên mà tại đây sẽ lọc 1 lần nữa những ảnh bên trên để lấy ra những ảnh có label SELLER và ADDRESS.

	#### Text detect sử dụng model Paddle
	
	- Introduction
	
		PaddleOCR nhằm mục đích tạo ra các công cụ OCR đa ngôn ngữ, có ích trong việc đào tạo mô hình tốt hơn và áp dụng chúng vào thực tế.
	
		![image](https://user-images.githubusercontent.com/104813668/168458473-a3d61cae-11de-4e7e-b135-fa7095ec9172.png)
	
		![image](https://user-images.githubusercontent.com/104813668/168458482-ac208551-3b7f-404c-9ec0-9fa958a46e93.png)

	- Features 
	
		PaddleOCR hỗ trợ một loạt các thuật toán cutting-edge liên quan đến OCR và phát triển feature thực nghiệm mô hình/cách giải quyết PP-OCR và PP-Structure, dựa trên cơ sở này nó cung cấp giải pháp tốt cho quá trình chuẩn bị data, model training, compression, inference and deployment.
	
		![image](https://user-images.githubusercontent.com/104813668/168458883-e13833ce-3191-4944-9f49-d8e005a178ca.png)
	
	- PP-OCR
	
		PP-OCR là hệ thống OCR có ích cho sự tự phát triển ultra-lightweight, nó được làm nhẹ và tối ưu dựa trên các thuật toán academic, dựa trên sự cân bằng giữa độ chính xác (accuracy) và thời gian chạy (speed). 
	
		Nó là hệ thống OCR 2 giai đoạn, bao gồm phần thuận toán text detection là DB, và phần thuật toán text recognition là CRNN. Tuy nhiên, phần text recognition sẽ không được nhóm nhắc đến trong bài vì nhóm đã dùng một mô hình nhận dạng tiếng Việt tốt hơn là VietOCR (được nói kĩ hơn ở mục *).
	
		PP-OCR áp dụng 19 effective stategies từ 8 khía cạnh bao gồm lựa chọn và điều chỉnh mạng lưới backbone, thiết kế prediction head, tăng cường dữ liệu, chuyển đổi learning rate, lựa chọn tham số phù hợp, sử dụng mô hình pre-training và tự động điều chỉnh mô hình, định lượng để tối ưu hóa và giảm dần các mô hình của mỗi mô-đun.
	
			- Thuật toán DB
		
			Sử dụng backbone: ResNet50_vd / MobileNetV3 (Ref: https://github.com/PaddlePaddle/PaddleOCR/blob/release/2.5/doc/doc_en/algorithm_det_db_en.md)
		
	- PaddleOCR text detection format annotation
	
		Định dạng tệp annotation được hổ trợ bởi thuật toán text detection PaddleOCR có dạng như sau, được chia bởi "\t":
		
			" Image file name             Image annotation information encoded by json.dumps"
			ch4_test_images/img_61.jpg    [{"transcription": "MASA", "points": [[310, 104], [416, 141], [418, 216], [312, 179]]}, {...}]	
		
		The image annotation sau khi mã hóa json.dumps() là một danh sách chứa nhiều dictionaries. 
	
		"points" trong dictionary đại diện cho tọa độ (x,y) của 4 điểm text box.
	
		"transciption" đại diện cho text của text box hiện tại.
	
3. Rotation corrector
4. Textline rotation
5. Text recognition

	#### VietOCR

4. Key information extraction

	#### KIE Model (PICK) 



