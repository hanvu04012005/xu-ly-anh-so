# Nhập Môn Xử Lý Ảnh Số – Lab 4

**Chủ đề:** Phân vùng ảnh xám 

**Sinh viên thực hiện:**  Nguyễn Hạn Vũ  **MSSV:** 2374802010571
**Môn học:** Nhập môn Xử lý ảnh số

**Giảng viên:** Thầy Đỗ Hữu Quân

---

##  Mục tiêu bài Lab

Trong bài Lab này, sinh viên thực hành các bước xử lý ảnh cơ bản để phân vùng ảnh , từ đó tách đối tượng khỏi nền dựa trên cường độ xám. Các bước bao gồm: chuyển đổi sang ảnh xám, hiển thị histogram, ngưỡng hóa, trích vùng quan tâm, làm mịn ảnh và tạo mặt nạ để làm nổi bật vùng mong muốn.

---

## 🛠 Công cụ sử dụng

* **Python** – ngôn ngữ chính
* **NumPy** – xử lý mảng ảnh
* **ImageIO** – đọc ảnh
* **Matplotlib** – hiển thị ảnh và histogram
* **SciPy (ndimage)** – xử lý ảnh nâng cao

---

##  Các bước thực hiện và phân tích lý thuyết

### 1. Ảnh xám *(Grayscale Image)*

####  Định nghĩa:

Ảnh xám là ảnh chỉ có một kênh màu, với giá trị pixel trong khoảng **0–255**, đại diện cho độ sáng.

####  Tác dụng:

* Giảm số kênh từ 3 (RGB) còn 1 → nhẹ hơn khi xử lý
* Dễ áp dụng các thuật toán phân vùng, như ngưỡng hóa

####  Công thức chuyển ảnh màu sang xám:

```python
gray = img.mean(axis=2).astype(np.uint8)
```

📷 Ảnh minh họa (ảnh bạn gửi): Chuyển đổi ảnh màu đầu vào sang dạng xám rõ nét, thấy rõ đối tượng và nền.

---

### 2. Biểu đồ Histogram *(Histogram)*

####  Định nghĩa:

Histogram thể hiện **tần suất xuất hiện của các mức xám** trong ảnh (0–255).

####  Tác dụng:

* Cho biết phân bố độ sáng trong ảnh
* Giúp xác định **ngưỡng phân vùng phù hợp**

 Khi ảnh có **2 đỉnh rõ ràng** → có thể chọn **ngưỡng ở giữa** để phân vùng ảnh.

####  Lệnh hiển thị Histogram:

```python
plt.hist(gray.ravel(), bins=256)
```

 Ảnh minh họa: Biểu đồ histogram từ ảnh xám, thấy rõ hai vùng đậm và nhạt.

---

### 3. Ngưỡng hóa nhị phân *(Binary Thresholding)*

#### Định nghĩa:

Dựa vào một giá trị ngưỡng `T`, chuyển mỗi pixel thành **đen hoặc trắng**.

####  Công thức toán học:

```
f(x, y) = 255 nếu I(x, y) >= T
         0   nếu I(x, y) < T
```

####  Tác dụng:

* Phân biệt nền và đối tượng rõ ràng
* Là bước tiền xử lý quan trọng trong segmentation

```python
binary = gray > T  # T là ngưỡng, ví dụ T=128
```

📷 Ảnh minh họa: Ảnh đen trắng rõ nét các vùng.

---

### 4. Trích xuất vùng quan tâm *(ROI – Region of Interest)*

####  Định nghĩa:

Lấy một phần của ảnh mà ta cần phân tích, bỏ vùng thừa.

####  Tác dụng:

* Giảm nhiễu và khối lượng tính toán
* Tập trung xử lý vào khu vực chính

```python
roi = gray[y1:y2, x1:x2]
```

 Ảnh minh họa: Chỉ giữ lại vùng đối tượng (hộp, sản phẩm...).

---

### 5. Làm mịn ảnh *(Smoothing / Blurring)*

####  Định nghĩa:

Làm mượt biên ảnh và giảm nhiễu bằng các bộ lọc trung bình hoặc Gaussian.

####  Tác dụng:

* Loại bỏ điểm nhiễu nhỏ
* Làm rõ biên phân vùng hơn sau thresholding

```python
from scipy.ndimage import gaussian_filter
smooth = gaussian_filter(binary.astype(float), sigma=1)
```

 Ảnh minh họa: Ảnh được làm mịn, các cạnh không còn bị răng cưa.

---

### 6. Tạo mặt nạ *(Masking)*

####  Định nghĩa:

Tạo mặt nạ nhị phân (0 và 1) để **lọc và giữ lại vùng mong muốn** từ ảnh gốc.

####  Tác dụng:

* Giữ lại đối tượng, xóa nền
* Dùng trong nhiều bài toán như OCR, phát hiện vật thể

```python
mask = smooth > 0.5
result = gray * mask
```

 Ảnh minh họa: Ảnh sau khi áp mặt nạ, chỉ giữ lại đối tượng chính.

---

##  Cách chạy chương trình

1. Cài thư viện cần thiết:

```bash
pip install numpy matplotlib imageio scipy
```

2. Mở file `main.ipynb` trong VSCode hoặc Jupyter Notebook
3. Chạy từng cell và quan sát kết quả ở mỗi bước
4. Có thể thay đổi giá trị ngưỡng, vùng ROI, sigma làm mịn để thấy ảnh hưởng

---

##  Tài liệu tham khảo

* Slide thực hành môn học – Trường Đại học Văn Lang
* Sách “Digital Image Processing” – Gonzalez & Woods

