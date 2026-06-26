# Kth Element (Phần tử thứ K)

> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
Chỉ cần **đúng phần tử thứ K** (không cần cả K phần tử). Là trường hợp đặc biệt của [[Top K Elements]].

## 2. Tư duy cốt lõi
- Giữ Min-Heap kích thước K → sau khi duyệt hết, **đỉnh `heap.top()` chính là phần tử thứ K lớn nhất**.
- Với dòng dữ liệu (stream): mỗi lần thêm phần tử, đỉnh luôn cập nhật ngay → trả về tức thì.
- **So sánh QuickSelect:** QuickSelect cho $O(n)$ trung bình nhưng phá vỡ khi cần stream. Heap thắng khi dữ liệu **liên tục chảy vào**.

## 3. Template
```cpp
#include <queue>
#include <vector>
using namespace std;

class KthLargest {
    int k;
    priority_queue<int, vector<int>, greater<int>> heap;   // Min-Heap size K
public:
    KthLargest(int k, vector<int>& nums) : k(k) {
        for (int x : nums) add(x);
    }
    int add(int val) {
        heap.push(val);
        if ((int)heap.size() > k)
            heap.pop();
        return heap.top();   // phần tử thứ K lớn nhất
    }
};
```

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Easy | [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) | Bài làm quen Min-Heap size K trên stream |
| Medium | [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Kinh điển — so sánh Heap vs QuickSelect |
| Medium | [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | Kết hợp ý tưởng Merge K (xem [[Merge K Sorted]]) |

## 5. Bẫy
- ❌ Tìm Kth **lớn** nhưng quên là phải dùng **Min**-Heap (tư duy ngược).
- ✅ Nếu chỉ chạy 1 lần trên mảng tĩnh và cần cực nhanh → QuickSelect $O(n)$.
