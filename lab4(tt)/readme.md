# Nhập Môn Xử Lý Ảnh Số – Lab 4

**Chủ đề:** Phân vùng ảnh xám 

**Sinh viên thực hiện:**  Nguyễn Hạn Vũ  
**MSSV:** 2374802010571
**Môn học:** Nhập môn Xử lý ảnh số

**Giảng viên:** Thầy Đỗ Hữu Quân

---

##  Mục tiêu bài Lab

Trong bài Lab này, em học cách xử lý ảnh để tách đối tượng khỏi nền dựa trên mức xám. Các bước gồm: chuyển sang ảnh xám, vẽ histogram, ngưỡng hóa ảnh thành trắng đen, cắt vùng cần thiết, làm mịn để giảm nhiễu và tạo mặt nạ làm nổi bật vùng mong muốn. Qua đó, em hiểu rõ hơn quy trình phân đoạn ảnh cơ bản.

---

##  Công cụ sử dụng

* **Python** – ngôn ngữ chính
* **NumPy** – xử lý mảng ảnh
* **ImageIO** – đọc ảnh
* **Matplotlib** – hiển thị ảnh và histogram
* **SciPy (ndimage)** – xử lý ảnh nâng cao

---

##  Các bước thực hiện và phân tích lý thuyết

### 1. Ảnh xám *(Grayscale Image)*

####  Định nghĩa:

Ảnh xám là ảnh mà mỗi điểm ảnh (pixel) chỉ có một giá trị độ sáng, nằm trong khoảng từ 0 đến 255. Giá trị này cho biết pixel đó sáng hay tối, càng gần 0 thì càng đen, càng gần 255 thì càng trắng. Ảnh xám đơn giản hơn ảnh màu nên em dễ xử lý và áp dụng các kỹ thuật như ngưỡng hóa hay lọc ảnh.

####  Tác dụng:

* Ảnh xám chỉ còn 1 kênh thay vì 3 kênh màu (RGB), nên nhẹ và xử lý nhanh hơn.
* Dễ dùng cho các kỹ thuật như ngưỡng hóa hay phân đoạn vì chỉ cần làm việc với 1 giá trị độ sáng.


####  Công thức chuyển ảnh màu sang xám:

```python
gray = img.mean(axis=2).astype(np.uint8)
```

---

### 2. Biểu đồ Histogram *(Histogram)*

####  Định nghĩa:

Histogram là biểu đồ cho thấy trong ảnh có bao nhiêu pixel ở mỗi mức xám, từ 0 (đen nhất) đến 255 (trắng nhất). Nhờ đó, em có thể biết ảnh sáng hay tối, có phân bố đều không, và dễ chọn ngưỡng để tách đối tượng khỏi nền.


####  Tác dụng:

* Cho biết phân bố độ sáng trong ảnh
* Giúp xác định **ngưỡng phân vùng phù hợp**

 Khi ảnh có 2 đỉnh rõ ràng → có thể chọn ngưỡng ở giữa để phân vùng ảnh.

####   Histogram:

```python
plt.hist(gray.ravel(), bins=256)
```

---

### 3. Ngưỡng hóa nhị phân *(Binary Thresholding)*

#### Định nghĩa:

Dựa vào một giá trị ngưỡng T, em sẽ so sánh từng pixel trong ảnh xám: nếu pixel sáng hơn hoặc bằng T thì cho nó thành màu trắng, còn nếu tối hơn thì cho thành màu đen. Như vậy, ảnh sẽ chỉ còn 2 màu đen và trắng, giúp em dễ dàng tách được vùng đối tượng ra khỏi nền. Đây là cách đơn giản nhưng rất hiệu quả để phân vùng ảnh.

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


---

### 4. Trích xuất vùng quan tâm *(ROI – Region of Interest)*

####  Định nghĩa:

Lấy một phần của ảnh mà em muốn tập trung xử lý, còn những vùng không cần thiết thì bỏ đi. Cách này giúp giảm nhiễu, tiết kiệm thời gian xử lý và chỉ phân tích đúng vùng em quan tâm, ví dụ như phần chứa đối tượng chính.

####  Tác dụng:

* Giảm nhiễu và khối lượng tính toán
* Tập trung xử lý vào khu vực chính

```python
roi = gray[y1:y2, x1:x2]
```

---

### 5. Làm mịn ảnh *(Smoothing / Blurring)*

####  Định nghĩa:

Làm mượt ảnh nghĩa là giúp các vùng chuyển màu trở nên mịn hơn, không bị gắt hoặc răng cưa. Em dùng các bộ lọc như trung bình hoặc Gaussian để làm mờ nhẹ ảnh, từ đó giảm bớt nhiễu nhỏ và làm cho đường biên của đối tượng rõ ràng, dễ xử lý hơn trong các bước tiếp theo.


####  Tác dụng:

* Loại bỏ điểm nhiễu nhỏ
* Làm rõ biên phân vùng hơn sau thresholding

```python
from scipy.ndimage import gaussian_filter
smooth = gaussian_filter(binary.astype(float), sigma=1)
```

---

### 6. Tạo mặt nạ *(Masking)*

####  Định nghĩa:

Tạo mặt nạ nhị phân tức là tạo một ảnh chỉ gồm các giá trị 0 và 1, trong đó số 1 là vùng em muốn giữ lại (đối tượng), còn số 0 là vùng cần bỏ (nền). Sau đó, em nhân mặt nạ này với ảnh gốc để chỉ hiển thị phần đối tượng, giúp làm nổi bật đúng vùng cần phân tích.

####  Tác dụng:

* Giữ lại đối tượng, xóa nền
* Dùng trong nhiều bài toán như OCR, phát hiện vật thể

```python
mask = smooth > 0.5
result = gray * mask
```

---


##  Tài liệu tham khảo

* Slide thực hành môn học – Trường Đại học Văn Lang
* Sách “Digital Image Processing” – Gonzalez & Woods

