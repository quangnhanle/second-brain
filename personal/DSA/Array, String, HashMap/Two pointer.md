## **1. Hai con trỏ đối xứng (Opposite Direction)**

- Thường dùng khi mảng đã được **sắp xếp**. Một con trỏ bắt đầu từ đầu (`left`), một con trỏ từ cuối (`right`).
- **Dạng bài:** Tìm cặp phần tử có tổng bằng K (Two Sum II), Đảo ngược mảng/chuỗi, Kiểm tra chuỗi đối xứng (Palindrome), Container With Most Water.
- **Hướng tư duy:** Dựa vào tổng hiện tại so với mục tiêu để quyết định dịch chuyển con trỏ nào.
    - Nếu `sum < target`: Tăng `left` để tăng tổng.
    - Nếu `sum > target`: Giảm `right` để giảm tổng.

## **2. Hai con trỏ cùng chiều (Same Direction / Sliding Window)**

- Cả hai con trỏ cùng bắt đầu từ đầu mảng nhưng di chuyển với **tốc độ** hoặc **điều kiện** khác nhau.
- **Dạng bài:** Loại bỏ phần tử trùng lặp (**Remove Duplicates**), Di chuyển số 0 về cuối mảng (**Move Zeroes**), Tìm chuỗi con dài nhất có K ký tự khác nhau.
- **Hướng tư duy:** Con trỏ `fast` dùng để duyệt qua tất cả phần tử, con trỏ `slow` dùng để ghi lại vị trí thỏa mãn điều kiện hoặc đánh dấu biên của "cửa sổ".

## **3. Con trỏ nhanh và chậm (Fast & Slow Pointers / Floyd's Cycle)**

- Thường dùng trong cấu trúc dữ liệu **Linked List**.
- **Dạng bài:** Tìm điểm giữa của Linked List, Kiểm tra vòng lặp (Cycle Detection), Tìm điểm bắt đầu của vòng lặp.
- **Hướng tư duy:** `fast` đi 2 bước, `slow` đi 1 bước. Nếu có vòng lặp, chắc chắn chúng sẽ gặp nhau.

## **4. Partition / Dutch National Flag (3-Way Partition)**

- Đây là một dạng đặc biệt của Two Pointers, thực tế thường dùng tới **3 con trỏ** để phân loại mảng thành 3 phần riêng biệt.
- **Bối cảnh:** Sắp xếp mảng chỉ gồm 3 loại phần tử (ví dụ: 0, 1, 2).
- **Cơ chế:**
    - `low`: Biên cho nhóm 0.
    - `mid`: Con trỏ duyệt.
    - `high`: Biên cho nhóm 2.
- **Tư duy:** Khi `mid` gặp số 0, swap với `low`. Khi gặp số 2, swap với `high`.
- **Ứng dụng thực tế:** Được dùng trong thuật toán **QuickSort** (biến thể Dual-Pivot) để xử lý các phần tử trùng lặp, giúp tránh độ phức tạp $O(n^2)$ trong trường hợp xấu nhất.

## **5. Merging Two Sequences**

- Đây là kỹ thuật cốt lõi trong **Merge Sort** và xử lý dữ liệu lớn (External Merge Sort).
- **Bối cảnh:** Trộn hai danh sách đã sắp xếp thành một danh sách duy nhất vẫn giữ nguyên thứ tự.
- **Cơ chế:** Hai con trỏ `i` và `j` đặt tại đầu mỗi danh sách. So sánh `A[i]` và `B[j]`, phần tử nào nhỏ hơn thì đưa vào kết quả và dịch con trỏ đó lên.
- **Kiến trúc hệ thống:** Kỹ thuật này cực kỳ quan trọng trong **LSM-Tree** (cấu trúc lưu trữ của các DB như Cassandra, RocksDB) khi thực hiện quá trình _Compaction_ để gộp các file dữ liệu đã sắp xếp.

## **6. Two Pointers + Condition Check**

- Đây thực chất là tên gọi chung cho các bài toán mà việc di chuyển con trỏ phụ thuộc vào một điều kiện logic phức tạp (không chỉ là so sánh lớn/bé).
- **Dạng bài tiêu biểu:**
    - **Valid Palindrome (sau khi loại bỏ ký tự đặc biệt):** Một con trỏ kiểm tra từ đầu, một từ cuối, nhảy qua các ký tự không phải chữ cái.
    - **Longest Substring with At Most K Distinct Characters:** Một con trỏ mở rộng vùng, một con trỏ co hẹp vùng dựa trên điều kiện số lượng ký tự trong HashMap.
    - **Tư duy:** Luôn giữ một "bất biến" (invariant). Ví dụ: "Đoạn từ `left` đến `right` luôn phải thỏa mãn điều kiện X".

## **Lộ trình bài tập thực hành (Tăng dần độ khó)**

| **Độ khó** | **Tên bài toán (LeetCode)**                                                                                   | **Loại**            | **Mục tiêu**                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------- |
| Easy       | [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)    | Đối xứng            | Nắm vững cách dịch chuyển con trỏ trên mảng đã sort.                                                          |
| Easy       | [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | Cùng chiều          | Hiểu cách dùng một con trỏ để "ghi đè" dữ liệu.                                                               |
| Medium     | [15. 3Sum](https://leetcode.com/problems/3sum/)                                                               | Đối xứng            | Kết hợp vòng lặp và Two Pointers, xử lý trùng lặp kết quả.                                                    |
| Medium     | [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)                     | Đối xứng            | Tư duy tham lam (Greedy) khi dịch chuyển con trỏ.                                                             |
| Medium     | [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)                              | Fast & Slow         | Kỹ thuật tìm điểm bắt đầu của vòng lặp (toán học).                                                            |
| Medium     | [75. Sort Colors](https://leetcode.com/problems/sort-colors/)                                                 | Dutch National Flag | Kinh điển.                                                                                                    |
| Easy       | [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)                                   | Merging             | Yêu cầu làm tại chỗ trên mảng có sẵn - dùng Two Pointers đi ngược từ cuối mảng về đầu để tối ưu $O(1)$ space. |
| Medium     | [922. Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)                        | Condition Check     | Sắp xếp số chẵn vào vị trí chẵn, số lẻ vào vị trí lẻ.                                                         |