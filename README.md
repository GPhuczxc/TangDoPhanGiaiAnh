## Cách Thức Hoạt Động Của Ứng Dụng

### A. Chuẩn Bị và Cấu Hình Ứng Dụng
**Môi trường:**  
Ứng dụng sử dụng các thư viện sau:
- **Tkinter**: Giao diện người dùng.
- **Pillow**: Xử lý ảnh.
- **TensorFlow**: Mô hình học sâu (CNN).

**Mô Hình CNN (Residual Network):**  
Mô hình CNN được thiết kế để tăng độ phân giải ảnh bằng cách:
- Sử dụng **Residual Block** để giữ thông tin quan trọng trong ảnh.
- Sử dụng **UpSampling2D** để phóng to kích thước ảnh đầu ra.

### B. Các Chức Năng Chính Của Ứng Dụng

1. **Tải Mô Hình Đã Huấn Luyện**  
   Người dùng nhấn "Tải Mô Hình Đã Huấn Luyện" để chọn mô hình `.h5` đã được huấn luyện trước.  
   **Hoạt động:**
   - Mô hình được tải bằng `tf.keras.models.load_model`.
   - Mô hình được lưu trữ trong biến `self.model` và sẵn sàng để xử lý ảnh.

2. **Tải Ảnh Đầu Vào**  
   Người dùng nhấn "Tải Ảnh" để chọn ảnh từ máy tính.  
   **Hoạt động:**
   - Ảnh gốc được đọc bằng thư viện Pillow.
   - Ảnh được làm mờ với bộ lọc **GaussianBlur**.
   - Ảnh làm mờ được hiển thị trong khung **Ảnh Đầu Vào**.

3. **Tăng Độ Phân Giải và Độ Nét Ảnh**  
   Người dùng nhấn "Tăng Độ Nét & Độ Phân Giải" để cải thiện ảnh.  
   **Hoạt động:**
   - Nếu không có mô hình đã tải, ứng dụng sử dụng **Pillow** để tăng độ nét bằng `ImageEnhance.Sharpness` và bộ lọc **DETAIL**.
   - Nếu có mô hình đã tải, ảnh đầu vào được chuyển thành tensor (numpy array) và chuẩn hóa, sau đó được xử lý qua mô hình CNN để dự đoán ảnh chất lượng cao hơn.
   - Ảnh kết quả được hiển thị trong khung **Ảnh Đã Xử Lý**.

4. **Hiển Thị và Lưu Ảnh**  
   Ứng dụng hiển thị cả hai ảnh: ảnh đầu vào và ảnh đã qua xử lý. Độ phân giải ảnh được hiển thị dưới từng khung hình.  
   Người dùng có thể nhấn "Lưu Ảnh" để lưu ảnh đã xử lý dưới dạng tệp `.png`.

### C. Các Chức Năng Phụ Trợ

1. **Tải Lịch Sử Huấn Luyện**  
   Người dùng nhấn "Tải Lịch Sử Huấn Luyện" để chọn tệp `.csv` chứa thông tin loss và accuracy của quá trình huấn luyện.  
   **Hoạt động:**
   - Tệp `.csv` được đọc bằng pandas và lưu trong biến `self.history_data`.

2. **Vẽ Biểu Đồ Lịch Sử Huấn Luyện**  
   Sau khi tải lịch sử, người dùng nhấn "Vẽ Biểu Đồ Lịch Sử".  
   **Hoạt động:**
   - Ứng dụng sử dụng **matplotlib** để vẽ biểu đồ về:
     - **Loss** (mất mát) qua các epoch.
     - **Accuracy** (độ chính xác) qua các epoch.
   - Biểu đồ được hiển thị trong cửa sổ mới.

### Quy Trình Xử Lý Ảnh

1. **Chuẩn Bị Ảnh Đầu Vào:**
   - Nếu không có mô hình, ảnh được làm mờ để giảm chất lượng giả lập.
   - Nếu có mô hình, ảnh được resize về kích thước phù hợp với mô hình CNN.

2. **Xử Lý Bằng CNN:**
   - Ảnh đầu vào được đưa qua các tầng tích chập (**Convolutional Layers**).
   - Các **Residual Block** giúp bảo toàn chi tiết ảnh gốc.
   - **UpSampling2D** giúp tăng kích thước ảnh đầu ra.

3. **Tăng Cường Chi Tiết Ảnh:**
   - Nếu không sử dụng mô hình, ảnh được cải thiện độ sắc nét bằng **Pillow**.

4. **Hiển Thị Ảnh Đã Xử Lý:**
   - Ứng dụng hiển thị ảnh kết quả cùng với độ phân giải.

---

## Kết Quả Thực Nghiệm


**Mô Tả Kết Quả:**  
Kết quả thực nghiệm được đánh giá dựa trên khả năng của ứng dụng trong việc tăng độ phân giải và cải thiện độ sắc nét của ảnh. Các kết quả được ghi nhận qua hai trường hợp chính:

#### 1. Trường Hợp 1: Xử Lý Ảnh Bằng Các Phương Pháp Cơ Bản (Không Dùng Mô Hình CNN)

- **Ảnh Đầu Vào:**  
  Loại ảnh: Phong cảnh và chân dung, độ phân giải thấp (dưới 128x128).
  
- **Quy Trình Xử Lý:**  
  1. Ảnh được làm mờ bằng bộ lọc **Gaussian Blur** (giảm chất lượng).
  2. Ảnh được tăng cường bằng bộ lọc `ImageEnhance.Sharpness` và bộ lọc **DETAIL**.
  
- **Kết Quả:**
  - **Cải thiện độ sắc nét:**  
    Ảnh rõ nét hơn so với ảnh đầu vào mờ. Một số chi tiết nhỏ được làm nổi bật.
  - **Độ phân giải không thay đổi:**  
    Ảnh không được phóng to hay cải thiện độ phân giải thực tế.
    
- **Ưu điểm:**
  - Xử lý nhanh, không yêu cầu mô hình phức tạp.
  - Phù hợp với các ảnh nhỏ hoặc yêu cầu làm rõ chi tiết nhanh chóng.
  
- **Hạn chế:**
  - Không cải thiện độ phân giải thực sự.
  - Chất lượng ảnh chỉ cải thiện nhẹ, phụ thuộc vào bộ lọc.

#### 2. Trường Hợp 2: Xử Lý Ảnh Bằng Mô Hình CNN (Khi Tải Mô Hình Huấn Luyện Trước)

- **Ảnh Đầu Vào:**  
  Loại ảnh: Ảnh chất lượng thấp (64x64, 128x128) với chi tiết bị mờ.
  
- **Quy Trình Xử Lý:**  
  1. Ảnh được resize về kích thước cố định (128x128).
  2. Ảnh được xử lý qua mô hình CNN đã huấn luyện trên tập dữ liệu ảnh chất lượng cao (tăng độ phân giải gấp 2 lần hoặc hơn).
  
- **Kết Quả:**
  - **Cải thiện độ phân giải:**  
    Ảnh đầu ra có độ phân giải cao hơn, ví dụ: từ 128x128 lên 256x256. Các chi tiết như cạnh, kết cấu và màu sắc được tái tạo rõ ràng.
  - **Cải thiện độ sắc nét:**  
    Ảnh đã xử lý rõ ràng và sắc nét hơn so với ảnh gốc.
  - **Độ mượt của ảnh:**  
    Mô hình CNN tái tạo ảnh mềm mại, không gây nhiễu hạt như các phương pháp thông thường.
  
- **Ưu điểm:**
  - Cải thiện cả độ phân giải lẫn độ sắc nét.
  - Kết quả gần với ảnh gốc chất lượng cao (tùy thuộc vào mức độ huấn luyện mô hình).
  
- **Hạn chế:**
  - Yêu cầu mô hình đã được huấn luyện tốt trên tập dữ liệu tương tự.
  - Thời gian xử lý ảnh lâu hơn so với các phương pháp cơ bản.
