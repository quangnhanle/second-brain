## 1. Bản chất và Công thức cốt lõi (Prefix Sum)

### Định nghĩa

Cho mảng A, ta xây dựng mảng Prefix Sum P như sau:

- P[i] = A[0] + A[1] + ... + A[i]

Ý nghĩa:

- P[i] lưu tổng các phần tử từ đầu mảng đến chỉ số i

### Tính tổng đoạn con [i, j]

- Sum(i, j) = P[j] - P[i - 1]
- Với quy ước: P[-1] = 0

👉 Nhờ vậy, mỗi truy vấn tổng đoạn chỉ mất **O(1)**.

---

## 2. Các dạng bài Prefix Sum thường gặp

---

### Dạng 1: Basic Range Sum Queries (Truy vấn tĩnh)

**Mô tả**

- Mảng ít thay đổi
- Cần trả lời rất nhiều truy vấn tổng đoạn [L, R]

**Tư duy**

1. Xây dựng Prefix Sum trong O(n)
2. Mỗi truy vấn trả lời trong O(1)

**Áp dụng khi**

- Query nhiều
- Không có cập nhật mảng

---

### Dạng 2: Prefix Sum + HashMap

(Tìm mảng con thỏa điều kiện)

⚠️ Dạng **cực kỳ quan trọng trong phỏng vấn**

**Bối cảnh**

- Đếm số mảng con có tổng bằng K
- Hoặc tìm mảng con dài nhất có tổng bằng 0

**Tư duy cốt lõi**

- Khi duyệt đến chỉ số i, ta có `current_prefix_sum`
- Ta cần tìm một vị trí j < i sao cho:

```
current_prefix_sum - P[j] = K
=> P[j] = current_prefix_sum - K
```

**Giải pháp**

- Dùng HashMap để lưu số lần xuất hiện của từng Prefix Sum
- Vừa duyệt mảng, vừa tra cứu

**Độ phức tạp**

- Thời gian: O(n)
- Bộ nhớ: O(n)

---

### Dạng 3: Difference Array (Range Update)

Ngược lại với Prefix Sum.

**Bối cảnh**

- Cần cộng giá trị V cho tất cả phần tử từ L đến R
- Làm từng phần tử sẽ rất chậm: O(n)

**Tư duy**

Thay vì update trực tiếp, ta đánh dấu:

```
Diff[L]   += V
Diff[R+1] -= V
```

Sau khi thực hiện tất cả các update:

- Chạy Prefix Sum trên mảng Diff
- Thu được mảng kết quả cuối cùng

👉 Kỹ thuật này giảm update từ O(n) xuống O(1) mỗi lần.

---

### Dạng 4: 2D Prefix Sum (Ma trận)

**Ứng dụng**

- Xử lý ảnh
- Truy vấn dữ liệu bảng
- Game map, heatmap, analytics

**Bài toán**

- Tính tổng các phần tử trong hình chữ nhật con của ma trận

**Tư duy**

- Áp dụng nguyên lý bao hàm – loại trừ (Inclusion – Exclusion)
- Tiền xử lý giúp mỗi truy vấn chạy trong O(1)

---

## 3. Bài tập thực hành (theo lộ trình Architect)

|Độ khó|Bài toán (LeetCode)|Dạng|Ghi chú|
|---|---|---|---|
|Easy|303. Range Sum Query – Immutable|Cơ bản|Hiểu encapsulation của Prefix Sum|
|Medium|560. Subarray Sum Equals K|HashMap|Bài **quan trọng nhất**, must-know|
|Medium|525. Contiguous Array|Biến đổi dữ liệu|Đổi 0 → -1 để dùng Prefix Sum|
|Medium|1109. Corporate Flight Bookings|Difference Array|Range update thực tế|
|Medium|304. Range Sum Query 2D|2D Prefix Sum|Tư duy hình học + tối ưu|

---

## 4. Góc nhìn Architect: Vấn đề Overflow (Rất quan trọng)

### Rủi ro thực tế

- Mảng A dùng int 32-bit
- Prefix Sum cộng dồn rất nhanh vượt quá giới hạn 2^31 - 1

👉 Đây là lỗi **rất nguy hiểm trong hệ thống production**

### Nguyên tắc

- **LUÔN dùng 64-bit cho Prefix Sum**

|Ngôn ngữ|Kiểu nên dùng|
|---|---|
|C++|long long|
|Java|long|
|Python|int (tự động big integer)|

---