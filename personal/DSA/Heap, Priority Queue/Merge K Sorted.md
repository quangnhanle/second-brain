# Merge K Sorted (Gộp K danh sách đã sắp xếp)

> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
Gộp **K danh sách/mảng đã sắp xếp** thành một kết quả có thứ tự, hoặc tìm phần tử nhỏ thứ K trên nhiều nguồn đã sắp.

## 2. Tư duy cốt lõi
- Đưa **phần tử đầu của mỗi danh sách** vào Min-Heap (kích thước tối đa K).
- Liên tục **lấy nhỏ nhất ra**, rồi **nạp phần tử kế tiếp** của chính danh sách vừa lấy.
- Heap luôn chỉ giữ K ứng viên → tổng độ phức tạp $O(N \log k)$ với N là tổng số phần tử.

So với gộp đôi tuần tự ($O(Nk)$), heap nhanh hơn hẳn khi K lớn.

## 3. Template
```cpp
#include <queue>
#include <vector>
#include <array>
using namespace std;

// Merge K mảng. Mỗi phần tử heap: {giá trị, chỉ số list, chỉ số trong list}
vector<int> mergeKArrays(vector<vector<int>>& lists) {
    // Min-Heap theo giá trị (phần tử đầu của array<int,3>)
    priority_queue<array<int,3>, vector<array<int,3>>, greater<>> heap;
    for (int i = 0; i < (int)lists.size(); i++)
        if (!lists[i].empty())
            heap.push({lists[i][0], i, 0});

    vector<int> res;
    while (!heap.empty()) {
        auto [val, i, j] = heap.top(); heap.pop();
        res.push_back(val);
        if (j + 1 < (int)lists[i].size())
            heap.push({lists[i][j + 1], i, j + 1});
    }
    return res;
}
```

> **Mẹo:** lưu `{giá trị, index, ...}` (dùng `array`/`tuple`) để khi giá trị bằng nhau, heap so sánh tiếp `index` — tránh phải so sánh trực tiếp con trỏ/`ListNode`. Với danh sách liên kết, dùng comparator riêng so theo `node->val`.

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Medium | [355. Design Twitter](https://leetcode.com/problems/design-twitter/) | Merge K feeds bằng heap (design) |
| Medium | [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | Mỗi hàng là một list đã sắp |
| Hard | [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | "Trùm" của dạng này |
| Hard | [632. Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | Merge K + theo dõi min/max |

## 5. Bẫy
- ❌ Quên kiểm tra list rỗng trước khi push phần tử đầu.
- ❌ So sánh `ListNode*` trực tiếp khi giá trị bằng → thêm index đệm hoặc comparator so theo `val`.
- ✅ Nhớ `greater<>` để biến `priority_queue` thành Min-Heap (mặc định là Max-Heap).
