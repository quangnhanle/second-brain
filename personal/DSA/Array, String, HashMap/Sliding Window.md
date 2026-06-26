## 1. Phân loại Sliding Window

Có 2 dạng chính mà bạn cần phân biệt rõ:

### Dạng A: Cửa sổ có kích thước cố định (Fixed Size)

Bạn được cho một số $K$ cụ thể và yêu cầu tìm một tính chất nào đó trong mọi vùng độ dài $K$.

- **Ví dụ:** Tìm trung bình cộng lớn nhất của các mảng con độ dài 5.
- **Cách làm:**
    1. Tính giá trị cho $K$ phần tử đầu tiên.
    2. Trượt sang phải: **Cộng** phần tử mới vào, **Trừ** phần tử cũ ở đuôi ra.

### Dạng B: Cửa sổ có kích thước linh hoạt (Dynamic Size)

Đây là dạng khó hơn và hay gặp trong phỏng vấn nhất. Kích thước cửa sổ sẽ co dãn tùy theo điều kiện.

- **Ví dụ:** Tìm chuỗi con ngắn nhất có tổng lớn hơn hoặc bằng $S$.
- **Cách làm:**
    1. Dùng con trỏ `right` để mở rộng cửa sổ cho đến khi thỏa mãn điều kiện.
    2. Dùng con trỏ `left` để co hẹp cửa sổ lại (để tối ưu kết quả) cho đến khi không còn thỏa mãn điều kiện nữa.

---

## 2. Công thức "Vàng" (Template) cho Sliding Window

Để không bị rối khi code, bạn nên áp dụng cấu trúc sau cho dạng Dynamic Size:

Python

```python
def sliding_window(nums):
    left = 0
    res = 0
    current_state = 0 # Có thể là sum, count, hoặc HashMap
    
    for right in range(len(nums)):
        # 1. Thu nạp phần tử nums[right] vào trạng thái hiện tại
        current_state += nums[right]
        
        # 2. Kiểm tra nếu vi phạm (hoặc thỏa mãn) điều kiện
        while condition_violated(current_state):
            # 3. Thu hẹp cửa sổ bằng cách đẩy nums[left] ra
            current_state -= nums[left]
            left += 1
            
        # 4. Cập nhật kết quả tối ưu tại đây
        res = max(res, right - left + 1)
    return res
```

---

## 3. Các bài tập thực hành kinh điển

Tôi đã sắp xếp theo lộ trình tư duy để bạn luyện tập:

| **Độ khó** | **Tên bài toán (LeetCode)**                                                                                                        | **Mục tiêu luyện tập**                        |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| **Easy**   | [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)                                       | Thành thạo dạng **Fixed Size**.               |
| **Medium** | [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)                                         | Dạng **Dynamic Size** cơ bản nhất.            |
| **Medium** | [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Kết hợp Sliding Window với **HashMap/Set**.   |
| **Medium** | [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)             | Tư duy nâng cao về cách "giữ nhịp" cửa sổ.    |
| **Hard**   | [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)                                            | "Trùm cuối" của dạng này, hội tụ mọi kỹ năng. |
## 4. Góc nhìn của Architect: Khi nào dùng Sliding Window?

Khi thiết kế hệ thống, Sliding Window không chỉ là thuật toán giải đố mà là giải pháp cho:

- **Rate Limiting:** Giới hạn số lượng request một user có thể gửi trong 1 phút (Sliding Window Log hoặc Sliding Window Counter).
- **Networking:** Giao thức TCP sử dụng Sliding Window để quản lý lưu lượng (Flow Control), đảm bảo bên gửi không làm tràn bộ đệm bên nhận.
- **Monitoring:** Tính toán trung bình cộng của các chỉ số hệ thống (CPU, RAM) trong 5 phút gần nhất.