
# Nhập Môn Xử Lý Ảnh Số – Lab 5
**Chủ đề:** Xác định đối tượng trong ảnh  
**Sinh viên:** Nguyễn Hạn Vũ  
**MSSV:** 2374802010571  
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Thầy Đỗ Hữu Quân

---
##  Mục tiêu bài thực hành
- Viết chương trình gán nhãn cho từng vùng ảnh
- Tính các đặc trưng của từng vùng theo `regionprops`
- Thực hiện các thao tác xác định biên, góc, và thay đổi ảnh
---

##  Cài đặt thư viện
Cài đặt các thư viện cần thiết:
```bash
pip install numpy scipy matplotlib imageio opencv-python scikit-image
```

---
##  Nội dung thực hành
### 2.1. Gán nhãn ảnh
- Ảnh đầu vào là ảnh nhị phân.
- Sử dụng `threshold_otsu` để phân ngưỡng.
- Dùng `label()` để đánh số vùng ảnh.
- Sử dụng `regionprops()` để lấy thông tin:
  - `Area`
  - `Centroid`
  - `BoundingBox`
- Vẽ kết quả bằng `matplotlib.patches.Rectangle`.
---
### 2.2. Dò tìm cạnh theo chiều dọc
- Dùng phép `nd.shift()` để lấy hiệu ảnh theo trục `x` hoặc `y`.

---
### 2.3. Dò tìm cạnh bằng Sobel Filter
- Dùng Sobel theo trục `x`, `y` sau đó cộng lại.

---

### 2.4. Xác định góc của đối tượng
- Cài đặt bộ lọc Harris thủ công.
- Tính đạo hàm theo 2 trục, sau đó làm trơn bằng Gaussian.
- Áp dụng công thức:

  ```python
  R = detC - alpha * (traceC)**2
  ```
#### 2.5.1. Hough Line Transform thủ công
- Áp dụng công thức rho = x*cos(theta) + y*sin(theta)
- Tạo ma trận Hough chứa điểm ảnh tương ứng với mỗi góc và khoảng cách
- Lưu trữ và hiển thị ảnh Hough không gian

#### 2.5.2. Harris Corner bằng thư viện
- Đọc ảnh và chuyển sang grayscale
- Sử dụng `corner_harris` để phát hiện góc
- Trực quan hóa điểm góc bằng `imshow`

---

### 2.6. Image Matching

- Sử dụng corner_harris để xác định điểm đặc trưng
- Trích xuất patch vuông quanh điểm đặc trưng (kích thước 11x11)
- Tính mô tả đặc trưng bằng cách flatten patch
- Tính khoảng cách Euclidean giữa 2 tập đặc trưng
- Ánh xạ các điểm gần nhất và vẽ đường nối trên ảnh

---
##  Kết quả
- Tìm được đúng số lượng vùng đối tượng trong ảnh.
- Hiển thị được box bao quanh và tọa độ tâm.
- Phát hiện chính xác cạnh và góc của các hình học.

---
##  Tài liệu tham khảo
- Tài liệu thực hành PDF từ Trường ĐH Văn Lang


