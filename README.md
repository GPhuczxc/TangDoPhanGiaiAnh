1. Mô Hình CNN được huấn luyện thông qua các bước chi tiết sau:
A. Chuẩn Bị Dữ Liệu:
Dữ liệu huấn luyện (Train) và xác thực (Validation):

Ảnh độ phân giải thấp (Low Resolution - LR) và độ phân giải cao (High Resolution - HR) được tải từ các thư mục.
Các ảnh được resize về kích thước chuẩn (128, 128) và chuẩn hóa giá trị pixel về khoảng [0, 1].
Xử lý dữ liệu:

Tải ảnh từ thư mục bằng hàm load_images().
Chuyển đổi ảnh thành mảng numpy (NumPy array) để tương thích với TensorFlow.
Phân bổ dữ liệu:

Tập huấn luyện bao gồm dữ liệu từ DIV2K_train_LR_bicubic và DIV2K_train_HR.
Tập xác thực bao gồm dữ liệu từ DIV2K_valid_LR_bicubic và DIV2K_valid_HR.
B. Kiến Trúc và Tối Ưu Hóa Mô Hình:
Xây dựng mô hình CNN:
Mô hình CNN có cấu trúc Encoder-Decoder:

Encoder: Trích xuất đặc trưng bằng các tầng tích chập (Convolution), chuẩn hóa (BatchNormalization) và giảm kích thước (MaxPooling).
Decoder: Tăng kích thước ảnh qua các tầng UpSampling kết hợp Convolution để tái tạo ảnh HR từ ảnh LR.
Thông số kỹ thuật:

Optimizer: Adam với learning rate 1e-4.
Loss function: Mean Squared Error (MSE).
Metrics: Đo lường độ chính xác (accuracy).
C. Quá Trình Huấn Luyện:
Cấu hình callback:

TrainingProgress: Ghi nhận tiến trình và tính toán các chỉ số như Precision, Recall, và F1-Score trên tập xác thực.
EarlyStopping: Dừng sớm nếu lỗi (loss) trên tập xác thực không giảm sau 5 epoch liên tiếp.
ReduceLROnPlateau: Giảm learning rate khi loss không cải thiện trong 3 epoch, đảm bảo hội tụ tối ưu.
Huấn luyện mô hình:

Sử dụng model.fit() với các tham số:
batch_size=32
epochs=50 (tăng số epoch nếu cần khi áp dụng trên toàn bộ tập dữ liệu lớn).
Lưu trữ kết quả:

Mô hình được lưu vào file trained_model.h5.
Kết quả huấn luyện (loss, accuracy, precision, recall, f1-score) được lưu vào file training_history.csv.
Các biểu đồ loss và các chỉ số khác theo epoch được lưu vào thư mục training_plots.
2. Phương Pháp Tăng Độ Phân Giải Của Ảnh:
A. Tiền Xử Lý Ảnh Đầu Vào:
Ảnh đầu vào (LR) được resize về kích thước chuẩn (128, 128) và chuẩn hóa giá trị pixel về [0, 1].
Định dạng đầu vào: (batch_size, 128, 128, 3).
B. Quá Trình Tái Tạo Ảnh:
Trích xuất đặc trưng: Các tầng Conv2D và BatchNormalization trích xuất đặc trưng từ ảnh LR.

Giảm kích thước và học đặc trưng sâu (Downsampling):
Sử dụng tầng MaxPooling2D để giảm kích thước không gian ảnh.

Tăng kích thước và tái tạo (Upsampling):
Các tầng UpSampling2D kết hợp với Conv2D tăng kích thước không gian ảnh từng bước.

Tầng cuối cùng sử dụng hàm kích hoạt sigmoid để giá trị pixel của ảnh HR đầu ra nằm trong khoảng [0, 1].

C. Dự Đoán Ảnh HR:
Dự đoán:
Đầu vào: Một ảnh LR hoặc tập ảnh LR.
Đầu ra: Ảnh HR được mô hình dự đoán.
Sử dụng hàm model.predict() để nhận ảnh HR đầu ra.

Hậu xử lý ảnh:
Chuyển đổi giá trị pixel từ [0, 1] về [0, 255] nếu cần hiển thị hoặc lưu ảnh. Lưu ảnh dự đoán bằng tf.keras.preprocessing.image.save_img().

D. Đánh Giá Chất Lượng Tái Tạo:
Chỉ số đánh giá:

So sánh ảnh HR dự đoán với ảnh HR thực trên các chỉ số:
MSE (Loss): Đánh giá độ sai khác giữa hai ảnh.
Precision, Recall, F1 Score: Đánh giá khả năng tái tạo chi tiết trong ảnh.
Trực quan hóa:
Hiển thị ảnh LR, HR thực và HR dự đoán để so sánh chất lượng.
