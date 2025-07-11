# Lab 5 – Nhập Môn Xử Lý Ảnh Số

**Chủ đề:** Xác định đối tượng trong ảnh  
**Sinh viên:** Nguyễn Hạn Vũ  
**MSSV:** 2374802010571  
**GV hướng dẫn:** Thầy Đỗ Hữu Quân  
---
##  Mục tiêu bài học
- Biết cách gán nhãn (label) cho từng vùng đối tượng trong ảnh  
- Tính được các đặc trưng cơ bản như: diện tích, tâm điểm, hộp bao quanh  
- Làm quen với các kỹ thuật tìm biên, xác định góc  
- Thực hành thêm về tìm đường thẳng và ghép đối tượng giữa hai ảnh
---
## thư viện cần cài

```bash
pip install numpy scipy matplotlib imageio opencv-python scikit-image
```
---
### 2.1. Gán nhãn cho vùng ảnh
- Dùng Otsu để phân ngưỡng, sau đó gán nhãn bằng `label()`  
- Lấy thông tin vùng bằng `regionprops()`  
- Vẽ hình chữ nhật quanh đối tượng
```python
from skimage import io, filters, measure, color
from matplotlib import pyplot as plt, patches
img = io.imread('exercise/objects.png', as_gray=True)
thresh = filters.threshold_otsu(img)
binary = img < thresh
label_img = measure.label(binary)
props = measure.regionprops(label_img)
fig, ax = plt.subplots()
ax.imshow(binary, cmap='gray')
for region in props:
    y, x = region.centroid
    minr, minc, maxr, maxc = region.bbox
    rect = patches.Rectangle((minc, minr), maxc - minc, maxr - minr, edgecolor='red', facecolor='none')
    ax.add_patch(rect)
    ax.plot(x, y, 'bo')

plt.show()
```
---
### 2.2. Dò cạnh 
```python
from scipy import ndimage
edge = binary.astype(int) - ndimage.shift(binary.astype(int), (0, 1))
plt.imshow(edge, cmap='gray')
plt.title("Cạnh theo chiều ngang")
plt.show()
```
---
### 2.3. Dò cạnh bằng Sobel
```python
from skimage import filters
sx = filters.sobel_h(binary)
sy = filters.sobel_v(binary)
sobel_total = sx + sy

plt.imshow(sobel_total, cmap='gray')
plt.title("Cạnh dùng Sobel")
plt.show()
```
---
### 2.4. Xác định góc của đối tượng bằng Harris thủ công
 Phần này, ta tự cài đặt thuật toán Harris để xác định góc của các đối tượng trong ảnh
- Tính đạo hàm ảnh theo 2 trục bằng Sobel
- Nhân chéo đạo hàm để tính ma trận C
- Làm trơn bằng bộ lọc Gaussian
- Áp dụng công thức tính điểm góc: `R = detC - alpha * (traceC)**2`
```python
from PIL import Image
import numpy as np
import scipy.ndimage as nd
import matplotlib.pyplot as plt
def Harris(indata, alpha=0.2):
    x = nd.sobel(indata, 0)
    y = nd.sobel(indata, 1)
    xl = x ** 2
    yl = y ** 2
    xy = abs(x * y)
    xl = nd.gaussian_filter(xl, 3)
    yl = nd.gaussian_filter(yl, 3)
    xy = nd.gaussian_filter(xy, 3)
    detC = xl * yl - 2 * xy
    trcC = xl + yl
    R = detC - alpha * trcC**2
    return R
data = Image.open('geometric.png')
bmg = Harris(data)
plt.imshow(bmg)
plt.show()
```
---
### 2.5.1. Dò tìm đường thẳng trong ảnh 

Ta thực hiện dò tìm các đường thẳng bằng cách tính toán giá trị `rho = x * cos(theta) + y * sin(theta)` và ghi nhận vào ma trận 
```python
def LineHough(data, gamma):
    V, H = data.shape
    R = int(np.sqrt(V * V + H * H))
    ho = np.zeros((R, 90), float)
    w = data + 0
    theta = np.arange(90)/180.0 * np.pi
    tp = np.arange(90).astype(float)
    ok = 1
    while ok:
        mx = w.max()
        if mx < gamma:
            ok = 0
        else:
            v,h = divmod(w.argmax(), H)
            y = V - v
            x = h
            rh = x * np.cos(theta) + y * np.sin(theta)
            for i in range(len(rh)):
                if 0 <= rh[i] < R and 0 <= tp[i] < 90:
                    ho[int(rh[i]), int(tp[i])] += mx
            w[v,h] = 0
    return ho
data = np.zeros((256, 256))
data[128, 128] = 1
bmg = LineHough(data, 0.5)
plt.imshow(bmg)
plt.show()
```
---

### 2.5.2. Dò tìm đường tròn trong ảnh 
Dùng thư viện `corner_harris` của `skimage.feature` để phát hiện các điểm đặc trưng như góc và cạnh, có thể ứng dụng trong dò tìm đường cong, bao gồm cả đường tròn.
```python
from skimage.feature import corner_harris
from skimage.color import rgb2gray
import matplotlib.pyplot as plt
import imageio.v2 as iio
data = iio.imread('bird.png')
image_gray = rgb2gray(data)
coordinate = corner_harris(image_gray, k=0.001)
plt.figure(figsize=(20,10))
plt.imshow(coordinate)
plt.axis('off')
plt.show()
```
---
### 2.6. Image Matching – Tìm điểm tương đồng giữa 2 ảnh
Ở phần này, ta tìm các điểm đặc trưng giữa 2 ảnh giống nhau hoặc gần giống để đối chiếu. 
- Dùng `corner_harris` để tìm điểm đặc trưng
- Lấy patch 11x11 xung quanh các điểm đó
- Tính mô tả đặc trưng bằng cách flatten patch
- Tính khoảng cách Euclidean và ghép cặp điểm gần nhất
- Vẽ kết quả nối các điểm tương đồng
```python
from skimage.feature import corner_peaks
from scipy.spatial.distance import cdist
def ensure_grayscale(img):
    if img.ndim == 3:
        return rgb2gray(img)
    return img
def detect_corners(image, min_dist=5):
    corners = corner_harris(image)
    coords = corner_peaks(corners, min_distance=min_dist)
    return coords
def extract_patch(img, coord, size=11):
    half = size // 2
    y, x = coord
    if y - half < 0 or x - half < 0 or y + half >= img.shape[0] or x + half >= img.shape[1]:
        return None
    return img[y - half:y + half + 1, x - half:x + half + 1]
def compute_descriptors(img, keypoints, size=11):
    descriptors = []
    valid_pts = []
    for pt in keypoints:
        patch = extract_patch(img, pt, size)
        if patch is not None:
            descriptors.append(patch.flatten())
            valid_pts.append(pt)
    return np.array(descriptors), valid_pts
def match_descriptors(desc1, desc2):
    D = cdist(desc1, desc2)
    matches = np.argmin(D, axis=1)
    return matches
def draw_matches(img1, img2, pts1, pts2, matches):
    h1, w1 = img1.shape
    h2, w2 = img2.shape
    canvas = np.zeros((max(h1, h2), w1 + w2))
    canvas[:h1, :w1] = img1
    canvas[:h2, w1:] = img2
    fig, ax = plt.subplots(figsize=(12, 6))
    ax.imshow(canvas, cmap='gray')
    for i in range(len(pts1)):
        y1, x1 = pts1[i]
        y2, x2 = pts2[matches[i]]
        ax.plot([x1, x2 + w1], [y1, y2], 'r-', linewidth=0.5)
        ax.plot(x1, y1, 'bo', markersize=2)
        ax.plot(x2 + w1, y2, 'go', markersize=2)
    ax.axis('off')
    plt.title('So khớp điểm đặc trưng giữa hai ảnh')
    plt.show()
img1 = ensure_grayscale(iio.imread('label_output.jpg'))
img2 = ensure_grayscale(iio.imread('label_output.jpg'))
kp1 = detect_corners(img1)
kp2 = detect_corners(img2)
desc1, valid_pts1 = compute_descriptors(img1, kp1)
desc2, valid_pts2 = compute_descriptors(img2, kp2)
match_idx = match_descriptors(desc1, desc2)
draw_matches(img1, img2, valid_pts1, valid_pts2, match_idx)
```
---

##  Tài liệu tham khảo
- tài liệu xử lý ảnh
