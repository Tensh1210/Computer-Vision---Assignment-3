# Computer Vision Assignment 3
**Ghép ảnh toàn cảnh (Panoramic Image Stitching)**

> **Môn học:** Xử lý Ảnh Số và Thị Giác Máy Tính  
> **Giảng viên:** TS. Võ Thanh Hùng  

---

## 📋 Nội dung

### Pipeline ghép ảnh toàn cảnh
- Trích xuất đặc trưng cục bộ (SIFT và ORB)
- So khớp đặc trưng (FLANN + Lowe's ratio test / BFMatcher + Hamming)
- Ước lượng Homography bằng DLT + RANSAC
- Inverse warping và Feather blending
- Ghép 4 ảnh theo chiến lược từ giữa ra hai phía

### So sánh SIFT và ORB
- Chất lượng căn chỉnh và tính liên tục tại vùng chồng lấn
- Phạm vi bao phủ góc nhìn
- Tốc độ xử lý

### Tham khảo: OpenCV Stitcher
- Đối chiếu pipeline thủ công với `cv.Stitcher` (cylindrical projection, tự động)

---

## 🗂️ Cấu trúc repo

```
.
├── BTL3_ComputerVision.ipynb   # Notebook chính — toàn bộ pipeline
├── 20240409_095557.jpg         # hinh1
├── 20240409_095602.jpg         # hinh2
├── 20240409_095607.jpg         # hinh3
├── 20240409_095613.jpg         # hinh4
└── README.md
```

---

## 🛠️ Công nghệ
- Python 3.8+
- OpenCV 4.x
- NumPy, Matplotlib

---

## 🚀 Chạy code

```bash
# Cài đặt
pip install opencv-python numpy matplotlib jupyter

# Chạy notebook
jupyter notebook BTL3_ComputerVision.ipynb
```

> Hoặc upload toàn bộ file trong repo lên **Google Colab** rồi chọn **Runtime → Run all**.

---

## 📊 Kết quả chính

**SIFT:**  
Descriptor float 128 chiều, FLANN KD-tree, Lowe's ratio test (τ = 0.7) — bao phủ góc nhìn rộng hơn, thời gian 0.52s nhờ FLANN tối ưu tốt cho descriptor float

**ORB:**  
Descriptor nhị phân 256-bit, BFMatcher Hamming + crossCheck, top-100 — tỉ lệ inlier cao hơn (82–97%), thời gian 1.83s do BFMatcher duyệt hai chiều với 3000 keypoint

**So sánh:**

| Phương pháp | Thời gian | Inlier rate (bước 1/2/3) |
|-------------|-----------|--------------------------|
| SIFT | 0.52s | 83% / 72% / 61% |
| ORB | 1.83s | 97% / 95% / 82% |

Cả hai phương pháp đều căn chỉnh cấu trúc cảnh vật ổn định, không xuất hiện ghost rõ tại vùng chồng lấn. Pipeline thủ công (homography phẳng) cho panorama hẹp hơn `cv.Stitcher` do tích lũy biến dạng khi ghép nhiều bước.

---

## 📚 Tài liệu tham khảo
1. Võ Thanh Hùng, *Slide bài giảng CV*, HCMUT, 2024-2025
2. D. G. Lowe, *Distinctive Image Features from Scale-Invariant Keypoints*, IJCV, 2004
3. E. Rublee et al., *ORB: An Efficient Alternative to SIFT or SURF*, ICCV, 2011
4. Hartley & Zisserman, *Multiple View Geometry in Computer Vision*, 2nd Ed., 2004
5. [OpenCV Documentation](https://docs.opencv.org/4.x/)
