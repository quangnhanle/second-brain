## 1. Các dạng bài Frequency Map thường gặp

### Dạng 1: Kiểm tra sự tồn tại và tính tương quan (Anagrams/Sets)

Dùng để xác định xem hai tập hợp dữ liệu có “giống nhau” về mặt thành phần hay không, bất kể thứ tự.

- **Bối cảnh:** Kiểm tra xem hai chuỗi có là Anagram của nhau không.
- **Tư duy:** Duyệt chuỗi 1 để tăng tần suất (`map[char]++`), duyệt chuỗi 2 để giảm tần suất (`map[char]--`). Nếu cuối cùng Map toàn số 0, chúng là Anagram.

---

### Dạng 2: Đếm và Truy vấn (Top K Elements)

Kết hợp Frequency Map với các cấu trúc dữ liệu khác (như Heap hoặc Bucket Sort) để tìm các phần tử nổi bật.

- **Bối cảnh:** Tìm K phần tử xuất hiện nhiều nhất trong một danh sách.
- **Tư duy:**
    1. Dùng Map để đếm số lần xuất hiện: `value -> frequency`.
    2. Đưa các entry của Map vào một **Min-Heap** kích thước $K$ hoặc dùng **Bucket Sort** để lấy kết quả.

---

### Dạng 3: First Unique/Repeating Element

Tìm phần tử đầu tiên thỏa mãn điều kiện về số lần xuất hiện.

- **Bối cảnh:** Tìm ký tự không lặp lại đầu tiên trong chuỗi.
- **Tư duy:** Duyệt lần 1 để xây dựng Map tần suất. Duyệt lần 2 qua chuỗi, check Map để tìm ký tự đầu tiên có `frequency == 1`.

---

### Dạng 4: Frequency Map + Sliding Window (Hard)

Đây là sự kết hợp mạnh mẽ nhất. Map đóng vai trò là "bộ nhớ" của cửa sổ đang trượt.

- **Bối cảnh:** Tìm tất cả các biến thể của một chuỗi trong một chuỗi khác (All Anagrams in a String).
- **Tư duy:** Khi cửa sổ dịch sang phải, cập nhật Map bằng cách tăng tần suất phần tử mới vào và giảm tần suất phần tử cũ vừa ra.

---

## 2. Kỹ thuật tối ưu "Senior"

Là một Architect, bạn cần biết khi nào **không cần** dùng HashMap:

- **Fixed Character Set:** Nếu dữ liệu chỉ gồm 26 chữ cái tiếng Anh thường, hãy dùng một mảng `int[26]` thay vì `HashMap<Character, Integer>`. Mảng (Array) có tốc độ truy cập nhanh hơn và tốn ít bộ nhớ hơn nhiều do không có overhead của Hash function và Collision handling.

---

## 3. Lộ trình bài tập thực hành

| Độ khó | Tên bài toán (LeetCode)                                                                                      | Dạng bài          | Bài học rút ra                               |
| ------ | ------------------------------------------------------------------------------------------------------------ | ----------------- | -------------------------------------------- |
| Easy   | [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)                                           | Cơ bản            | Cách dùng mảng `int[26]` làm Frequency Map.  |
| Easy   | [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/) | Đếm 2 lần         | Tư duy duyệt 2 vòng để tìm kết quả.          |
| Medium | [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)                       | Map + Heap/Bucket | Cách kết hợp Map với cấu trúc dữ liệu khác.  |
| Medium | [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)             | Sắp xếp           | Biến đổi Map thành danh sách để xử lý.       |
| Medium | [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)           | Map + Window      | Đỉnh cao của việc quản lý trạng thái cửa sổ. |

---

## 4. Ứng dụng trong System Design

Frequency Map là cốt lõi của nhiều hệ thống lớn:

- **Search Engine:** Đếm tần suất từ khóa trong văn bản để tính điểm **TF-IDF** (độ liên quan của trang web).
- **Cache Eviction (LFU):** Thuật toán _Least Frequently Used_ sử dụng Map để theo dõi số lần truy cập dữ liệu và xóa bỏ phần tử ít được dùng nhất khi cache đầy.
- **Deduplication:** Loại bỏ dữ liệu trùng lặp trong các hệ thống lưu trữ lớn.