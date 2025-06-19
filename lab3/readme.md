BÀI 2:
####Công nghệ sử dụng
Framework: PIL, NumPy, matplotlib
	PIL: để mở ảnh và lưu ảnh.
	NumPy: dùng để tính toán nhanh, đặc biệt xử lý mảng (ma trận ảnh).
	matplotlib: để hiển thị ảnh kết quả.
	Dùng os: để chạy qua từng ảnh trong thư mục.
####Thuật toán sử dụng
Trong bài này dùng 3 thuật toán xử lý ảnh theo kiểu biến đổi tần số:
	Fast Fourier Transform : biến ảnh từ không gian ảnh sang không gian tần số
	Butter Low  Filter : giữ lại tần số thấp cho ra ảnh mịn hơn.
	Butter High  Filter : giữ lại tần số cao cho ra ảnh nét cạnh, rõ chi tiết nhỏ.
  Giải thích cách hoạt động
Các hàm xử lý ảnh sau khi convert ảnh sang ảnh xám (grayscale):
 ###Fast Fourier Transform:
Biến ảnh sang miền tần số. Dùng công thức log để lấy biên độ (magnitude), do FFT ra giá trị lớn nên phải log để nhìn rõ. Sau đó scale về dạng 0–255 để hiện ra được ảnh.
def fast_fourier(img):
   ![image](https://github.com/user-attachments/assets/b9d87c84-d031-407e-873c-f93319e89fa1)
 ###Butter Low:
	![image](https://github.com/user-attachments/assets/05f10d8c-7398-4d38-befe-645c4aa9b0a1)
 Tạo tròn ở giữa ảnh, giữ lại vùng trung tâm.
Giúp ảnh bị mờ đi, giảm nhiễu và rõ những chi tiết nhỏ
 ###Butter High  :
 ![image](https://github.com/user-attachments/assets/387d2237-f3d9-4742-85ed-640167e98b34)
	Ngược lại với Butter Low, bỏ đi phần trung tâm, giữ viền
 kết quả Làm ảnh nét hơn, giữ đường biên rõ
 BÀI 3:
  Thuật toán sử dụng
	I - Invert: đảo ảnh.
	G - Gamma: chỉnh độ sáng tối ảnh bằng công thức lũy thừa.
	L - Log: làm sáng vùng tối, thu nhỏ vùng sáng.
	H - Histogram : cân bằng độ sáng toàn ảnh.
	C - Contrast : tăng độ tương phản ảnh. Giải thích cách hoạt động
Sau khi ảnh được chuyển sang ảnh xám, chương trình sẽ chọn ngẫu nhiên 1 trong 5 cách dưới đây để xử lý:
Invert (I)
Công thức:
![image](https://github.com/user-attachments/assets/6a2d1c10-07e4-4669-9342-4ee64854a947)
 Giúp đổi màu ảnh: sáng thành tối, tối thành sáng
Gamma Correction (G)
Công thức:
![image](https://github.com/user-attachments/assets/7e03913a-2f83-4f38-be1b-a4cb99389eea)
 Làm ảnh sáng hơn hoặc tối đi tùy theo gamma đã cho
Logarithmic (L)
Công thức:
![image](https://github.com/user-attachments/assets/786aea70-dd09-43a4-ab99-dafd936b7e58)
 Làm nổi bật vùng tối, nén vùng sáng.
Histogram  (H
![image](https://github.com/user-attachments/assets/d4533517-213a-478e-8f73-535309cc64ef)
  Cách này dùng để cân bằng độ sáng cho ảnh.
 Phân bố lại các giá trị xám để làm rõ chi tiết ảnh.
Contrast (C)
![image](https://github.com/user-attachments/assets/0381542c-67eb-4b09-ad65-f1cbdbc13ece)
làm cho độ sángcủa ảnh thay đổi từ tối nhất đến sáng nhất ảnh rõ hơn
BÀI 4:
THUẬT TOÁN SỬ DỤNG
Chương trình xử lý ảnh với 3 bộ lọc tần số:
1. Fast Fourier Transform 
Dùng để đổi ảnh từ dạng bình thường sang dạng tần số 
2. Butter Low Filter + Min Filter
Cái này giữ lại phần tần số thấp như những thứ mềm, ít chi tiết, còn bỏ đi tần số cao như đường nét của ảnh.
3. Butter High Filter + Max Filter
Ngược lại cái trên, cái này giữ tần số cao như chi tiết rõ, cạnh sắc, bỏ phần trung tâm đi
 Fast Fourier Transform
![image](https://github.com/user-attachments/assets/bb7fbb4b-0d18-4dbd-95de-8fda388d8a45)
ảnh sẽ không giống ảnh gốc mà cho biết vùng nào nhiều thông tin/tần số hơn, vì thực tế thì filter này dừng để biến ảnh thành ttaanf số
Butter Low Filter
![image](https://github.com/user-attachments/assets/2ee42829-f422-4bbe-b772-7fe4552aaeb2)
	Tạo điểm tròn, ở giữa là 1, ngoài xa sẽ gần 0
	Sau khi xử lý lại ảnh làm mịn tiếp bằng minimum_filter.
![image](https://github.com/user-attachments/assets/c15655be-2453-42b6-8299-ac634a544853)
 làm ảnh trơn, mịn như kiểu làm mờ để che bớt nhiễu cho ra ảnh mịn hơn
 Butter High Filter
![image](https://github.com/user-attachments/assets/0e175f26-0cf4-4f5e-ad65-88fa94005a7f)
	Dùng để làm rõ các nét và chi tiết nhỏ trong ảnh.
	Cuối cùng lọc qua maximum_filter để tăng độ sắc nét hơn cho ảnh
![image](https://github.com/user-attachments/assets/64a15a6b-8047-4e39-9284-eeddd9c9b81b)
 dùng để làm ảnh rõ đường viền.







