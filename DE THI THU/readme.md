# BÀI THI THỬ – NHẬP MÔN XỬ LÝ ẢNH SỐ
- Họ tên: Nguyễn Hạn Vũ  
- MSSV: 2374802010571  
- Môn học: Nhập môn Xử lý ảnh số  
- Giảng viên hướng dẫn: Thầy Đỗ Hữu Quân  

---

## bài thi thử

Bài thi thử môn Nhập môn Xử lý ảnh số giúp em ôn tập lại các kiến thức đã học và áp dụng vào thực tế bằng ngôn ngữ Python và thư viện OpenCV. Nội dung bài thi gồm 3 câu, mỗi câu tập trung vào một nhóm kỹ thuật khác nhau trong xử lý ảnh:
Câu 1: Thực hiện các thao tác cơ bản trên một ảnh như làm mịn bằng bộ lọc trung bình, phát hiện biên, đổi màu ảnh theo cách ngẫu nhiên, và chuyển đổi không gian màu BGR sang HSV để tách các kênh màu.
Câu 2: Xây dựng một chương trình có menu động để áp dụng nhiều phương pháp biến đổi ảnh (log, gamma, histogram, CLAHE...) trên ba ảnh đầu vào cùng lúc, đồng thời lưu kết quả tự động.
Câu 3: Xử lý từng ảnh cụ thể theo yêu cầu như tăng kích thước ảnh, xoay và lật ảnh, làm mờ bằng Gaussian blur, và áp dụng công thức biến đổi tùy chỉnh.
Thông qua bài thi, em hiểu rõ hơn về quy trình xử lý ảnh từ cơ bản đến nâng cao, rèn luyện cách viết code rõ ràng, có cấu trúc và áp dụng linh hoạt các phương pháp đã học vào từng tình huống cụ thể.
---

##  Câu 1: Xử lý ảnh đơn giản (2 điểm)

Ảnh dùng: `a.jpg`

**Các bước cần làm:**

- Lọc trung bình ảnh để làm mịn ảnh gốc
- Phát hiện biên trong ảnh (sử dụng Sobel hoặc Laplacian)
- Đổi màu ảnh bằng cách tráo ngẫu nhiên các kênh màu RGB
- Chuyển ảnh sang không gian HSV và tách 3 kênh (Hue, Saturation, Value) thành ảnh xám

**Kết quả lưu ra:**

- `a_mean.jpg`
- `a_edge.jpg`
- `a_random_color.jpg`
- `a_hue.jpg`, `a_saturation.jpg`, `a_value.jpg`

---

## Câu 2: Menu động và xử lý nhiều ảnh (4 điểm)

Ảnh đầu vào: `image1.jpg`, `image2.jpg`, `image3.jpg`

**Yêu cầu:**

Tạo một chương trình cho phép người dùng bấm phím để chọn cách xử lý ảnh. Có 6 cách biến đổi:

- I: Đảo ảnh (Image inverse)
- G: Hiệu chỉnh Gamma (giá trị gamma ngẫu nhiên 0.5 – 2.0)
- L: Biến đổi Log (hệ số ngẫu nhiên 1.0 – 5.0)
- H: Cân bằng histogram thường
- C: Kéo giãn tương phản (contrast stretching)
- A: Cân bằng histogram thích nghi CLAHE (lưới 8x8)

Chương trình xử lý cả 3 ảnh cùng lúc và lưu kết quả theo tên:

```
output_[tên_biến_đổi]_[số_ảnh].jpg
```

Ví dụ: `output_inverse_2.jpg`, `output_gamma_3.jpg`

---

##  Câu 3: Biến đổi nâng cao (4 điểm)

Xử lý các ảnh khác nhau với các yêu cầu cụ thể:

- Ảnh `colorful-ripe-tropical-fruits.jpg`: tăng kích thước lên thêm 30 pixel chiều ngang và dọc
- Ảnh `quang-ninh.jpg`: xoay 45 độ thuận chiều kim đồng hồ rồi lật ngang
- Ảnh `pagoda.jpg`: phóng to 5 lần, làm mịn bằng Gaussian blur (kernel 7x7)
- Cuối cùng, áp dụng công thức điều chỉnh độ sáng và tương phản:

```
I_out(x, y) = α * I_in(x, y) + β
```

Với:
- α từ 0.5 đến 2.0 (điều chỉnh tương phản)
- β từ -50 đến +50 (điều chỉnh độ sáng)
- Kết quả cần được giới hạn trong khoảng [0, 255] để không bị lỗi ảnh

---



