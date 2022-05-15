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
		
	- Trong bài toán của nhóm, PaddleOCR sẽ detect vùng chữ của 4 trường thông tin [SELLER, ADDRESS, TIMESTAMP, TOTAL COST] trong hóa đơn từ ảnh thu được sau khi detect bằng YOLO.
	
	- PaddleOCR text detection format annotation (form mẫu)
	
		Định dạng tệp annotation được hổ trợ bởi thuật toán text detection PaddleOCR có dạng như sau, được chia bởi "\t":
		
			" Image file name             Image annotation information encoded by json.dumps"
			path_images/img.jpg    [{"transcription": "content-images", "points": [[310, 104], [416, 141], [418, 216], [312, 179]]}, {...}]	
		
		The image annotation sau khi mã hóa json.dumps() là một danh sách chứa nhiều dictionaries. 
	
		"points" trong dictionary đại diện cho tọa độ (x,y) của 4 điểm text box.
	
		"transciption" đại diện cho text của text box hiện tại.
	
3. Rotation corrector

	Xử lý ở bước tiền xử lý?

4. Textline rotation

	Sau khi xoay lại hóa đơn sẽ vẫn còn nhiều dòng chữ bị nghiêng, bước này sẽ cắt vùng chữ đó và xoay lại cho thẳng để phần OCR được tốt hơn.

5. Text recognition

	#### VietOCR

	Tác giả của VietOCR xây dựng thư viện VietOCR với mục đích giải quyết các bài toán liên quan đến OCR trong công nghiệp, đặc biệt nó rất hiệu quả đối với các bài toán nhận diện chữ tiếng Việt. Vì vậy trong đề tài của nhóm quyết định chọn VietOCR cho phần nhận dạng chữ trên hóa đơn.
	
	Thư viện VietOCR được mình xây dựng với mục đích hỗ trợ các bạn có thể sử dụng để giải quyết các bài toán liên quan đến OCR trong công nghiệp. Thư viện cung cấp cả 2 kiến trúc AtentionOCR và TransformerOCR. Tuy kiến trúc TransformerOCR hoạt động khá tốt trong NLP, nhưng theo mình nhận xét thì độ chính không có sự cải thiện đáng kể so với AttentionOCR mà thời gian dự đoán lại chậm hơn khá nhiều.
	
	- MÔ HÌNH
		
		Kết hợp mô hình CNN và mô hình Language Model (Seq2Seq và Transformer) để tạo thành một mô hình giải quyết bài toán OCR.
		
		- Seq2Seq


			Mô hình Sequence-to-Sequence là mô hình encoders-decoders, nó thường sử dụng Recurrent Neural Network (RNN) để mã hóa đầu vào thành một single vector (single vector ở đây là một context vector). Context vector này sau đó sẽ được giải mã bởi RNN thứ 2, RNN này được học để xuất ra câu mục tiêu.
				
				![image](https://user-images.githubusercontent.com/104813668/168463669-a22cc87f-b237-4ade-a939-0a8cf631f5b4.png)
						
			
	
		- CNN của mô hình OCR 
		
			Mô hình CNN dùng trong bài toán OCR nhận đầu vào là một ảnh, thông thường có kích thước với chiều dài lớn hơn nhiều so với chiều rộng, do đó việc điều chỉnh tham số stride size của tầng pooling là cực kì quan trọng. Mình thường chọn kích thước stride size của các lớp pooling cuối cùng là wxh=2x1 trong mô hình OCR. Không thay đổi stride size phù hợp với kích thước ảnh thì sẽ dẫn đến kết quả nhận dạng của mô hình sẽ tệ.

			Đối với mô hình VGG, việc thay đổi pooling size khá dễ do kiến trúc đơn giản, tuy nhiên đối với mô hình phức tạp khác như resnet việc điều chỉnh tham số pooling size hơi phức tạp do một ảnh bị downsampling không chỉ bởi tầng pooling mà còn tại các tầng convolution khác.

			Trong pytorch, đối với mô hình VGG, các bạn chỉ đơn giản là thay thế stride size của tầng pooling.

				cnn.features[i] = torch.nn.AvgPool2d(kernel_size=ks[pool_idx], stride=ss[pool_idx], padding=0)
			
		- AttentionOCR
			
			![image](https://user-images.githubusercontent.com/104813668/168462817-fc487956-3214-47cf-883a-ea969699d954.png)

			AttentionOCR là sự kết hợp giữa mô hình CNN và mô hình Attention Seq2Seq. Cách hoạt động của mô hình này tương tự như kiến trúc của mô hình seq2seq trong bài toán dịch máy. Với bài toán dịch máy từ tiếng việt sang anh, chúng ta cần encode một chuỗi tiếng việt thành một vector đặc trưng, còn trong mô hình AttentionOCR, thì dữ liệu đầu vào này là một ảnh.

			Một ảnh qua mô hình CNN, sẽ cho một feature maps có kích thước channelxheightxwidth, feature maps này sẽ trở thành đầu vào cho mô hình LSTM, tuy nhiên, mô hình LSTM chỉ nhận chỉ nhận đầu vào có kích thước là hiddenxtime_step. Một cách đơn giản và hợp lý là 2 chiều cuối cùng heightxwidth của feature maps sẽ được duổi thẳng. Feature maps lúc này sẽ có kích thước phù hợp với yêu cầu của mô hình LSTM.
			
			![image](https://user-images.githubusercontent.com/104813668/168462835-d0f63419-f6f5-450b-a2b0-04d600037c30.png)
			
			Feature maps của mô hình CNN sau khi được flatten thì được truyền vào làm input của mô hình LSTM, tại mỗi thời điểm, mô hình LSTM cần dự đoán từ tiếp theo trong ảnh là gì.
			
		- TransformerOCR

			![image](https://user-images.githubusercontent.com/104813668/168462852-187a8791-9109-4732-86ad-295f8eb7865b.png)

		- Huấn luyện mô hình 

			Huấn luyện mô hình AttenionOCR hay TransformerOCR hoàn toàn giống với luyện mô hình seq2seq, chúng đều sử dụng cross-entropy loss để tối ưu thay vì sử dụng CTCLoss như mô hình CRNN, tức là tại mỗi thời điểm mô hình dự đoán một từ sau đó so sánh với nhãn để tính loss và cập nhật lại trọng số của mô hình.
			
		- Hạn Chế Của Mô Hình Sử dụng CTCLoss

			Đối với mô hình CRNN sử dụng CTCloss để làm hàm mục tiêu, số lượng kí tự đối đa có thể dự đoán bằng với widthxheight của feature maps. Do đó, các bạn cần phải cẩn thận điều chỉnh kiến trúc mô hình để có thể dự đoán được số kí tự phù hợp với từng bộ dataset. Đối với mô hình AttentionOCR hoặc TransformerOCR, các bạn không gặp vấn đề này, làm cho các bạn có thể dễ dàng sử dụng lại pretrained model cho các loại dữ liệu khác nhau.
			
			Ngoài ra, AttentionOCR hoặc TransformerOCR đều có kiến trúc của mô hình dịch này seq2seq, do đó các thủ thuật của mô hình này đều có thể ứng dụng cho mô hình của chúng ta.
			
	- Dataset 

		...
		Cấu trúc thư mục chứa dữ liệu
			.
			├── img
			
			│   ├── 00000.jpg
			
			│   ├── 00001.jpg
			
			├── train_annotation.txt # nhãn tập train 
			
			└── val_annotation.txt # nhãn tập test
			
		Dữ liệu file nhãn theo định dạng sau
		
			path_to_file_name[tab]nhãn
			
			img/74086.jpg   429/BCT-ĐTĐL
			
			img/04225.jpg   Như trên;
			
			img/97822.jpg   V/v: Duyệt dự toán chi phí Ban QLDA nhiệt điện 1 năm 2012 và
			
	- Custom Augmentor

		Mặc định, mô hình có sử dụng augmentation, tuy nhiên các bạn có thể cần augmentate theo cách khác nhau để đảm bảo tính biến dạng ảnh không quá lớn so với dữ liệu gốc. Do đó, thư viện cho phép các bạn tự định nghĩa augmentation như ví dụ dưới, và truyền vào lúc huấn luyện.
		
		
			from vietocr.loader.aug import ImgAugTransform
			
			from imgaug import augmenters as iaa
			

			class MyAugmentor(ImgAugTransform):
			
			    def __init__(self):
			    
				self.aug = iaa.GaussianBlur(sigma=(0, 1.0))
				
	- Huấn Luyện

		Để huấn luyện mô hình các bạn chỉ cần tạo được bộ dataset của mình, sau đó thay đổi các tham số quan trọng là có thể huấn luyện mô hình dễ dàng.
		
			from vietocr.tool.config import Cfg
			
			from vietocr.model.trainer import Trainer
			

			# Các bạn có thể chọn vgg_transformer hoặc vgg_seq2seq 
			
			config = Cfg.load_config_from_name('vgg_transformer')
			

			# Các bạn có thể thay đổi tập vocab của mình hoặc để mặc định vì tập vocab của mình đã tương đối đầy từ các kí tự rồi 
			
			# lưu ý rằng các kí tự không có trong tập vocab sẽ bị lỗi
			
			#config['vocab'] = 'tập vocab'
			

			dataset_params = {
			
			    'name':'hw', # tên dataset do bạn tự đặt
			    
			    'data_root':'./data_line/', # thư mục chứa dữ liệu bao gồm ảnh và nhãn
			    
			    'train_annotation':'train_line_annotation.txt', # ảnh và nhãn tập train
			    
			    'valid_annotation':'test_line_annotation.txt' # ảnh và nhãn tập test
			    
			}

			params = {
			
				 'print_every':200, # hiển thị loss mỗi 200 iteration 
				 
				 'valid_every':10000, # đánh giá độ chính xác mô hình mỗi 10000 iteraction
				 
				  'iters':20000, # Huấn luyện 20000 lần
				  
				  'export':'./weights/transformerocr.pth', # lưu model được huấn luyện tại này
				  
				  'metrics': 10000 # sử dụng 10000 ảnh của tập test để đánh giá mô hình
				  
				 }

			# update custom config của các bạn
			
			config['trainer'].update(params)
			
			config['dataset'].update(dataset_params)
			
			config['device'] = 'cuda:0' # device để huấn luyện mô hình, để sử dụng cpu huấn luyện thì thay bằng 'cpu'

			# huấn luyện mô hình từ pretrained model của mình sẽ nhanh hội tụ và cho kết quả tốt hơn khi bạn chỉ có bộ dataset nhỏ
			
			# để sử dụng custom augmentation, các bạn có thể sử dụng Trainer(config, pretrained=True, augmentor=MyAugmentor()) theo ví dụ trên.
			
			trainer = Trainer(config, pretrained=True)

			# sử dụng lệnh này để visualize tập train, bao gồm cả augmentation 
			
			trainer.visualize_dataset()
			

			# bắt đầu huấn luyện 
			
			trainer.train()


			# visualize kết quả dự đoán của mô hình
			
			trainer.visualize_prediction()

			# huấn luyện xong thì nhớ lưu lại config để dùng cho Predictor
			
			trainer.config.save('config.yml')
			

	- Inference

		Sau khi huấn luyện mô hình các bạn sử dụng config.yml và trọng số đã huấn luyện để dự đoán hoặc chỉ sử dụng pretrained model của mình.
		 
			from vietocr.tool.predictor import Predictor
			
			from vietocr.tool.config import Cfg
			

			# config = Cfg.load_config_from_file('config.yml') # sử dụng config của các bạn được export lúc train nếu đã thay đổi tham số  
			
			config = Cfg.load_config_from_name('vgg_transformer') # sử dụng config mặc định của mình 
			
			config['weights'] = './weights/transformerocr.pth' # đường dẫn đến trọng số đã huấn luyện hoặc comment để sử dụng pretrained model của mình
			
			config['device'] = 'cuda:0' # device chạy 'cuda:0', 'cuda:1', 'cpu'
			

			detector = Predictor(config)
			

			img = './a.JPG'
			
			img = Image.open(img)
			
			# dự đoán 
			
			s = detector.predict(img, return_prob=False) # muốn trả về xác suất của câu dự đoán thì đổi return_prob=True
			
	- Kết Quả


6. Key information extraction

	#### KIE Model (PICK) 



