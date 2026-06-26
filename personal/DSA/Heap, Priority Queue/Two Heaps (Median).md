# Two Heaps (Hai Heap đối xứng)

> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
Tìm **median (trung vị)** của một dòng dữ liệu, hoặc bài cần liên tục truy cập "phần tử ở giữa", hoặc cân đối hai nửa dữ liệu theo ưu tiên.

## 2. Tư duy cốt lõi
Chia dữ liệu làm 2 nửa, mỗi nửa là một heap:
- **Max-Heap `small`** giữ **nửa nhỏ** → đỉnh là phần tử lớn nhất của nửa nhỏ (C++ `priority_queue` mặc định).
- **Min-Heap `large`** giữ **nửa lớn** → đỉnh là phần tử nhỏ nhất của nửa lớn.

Hai bất biến cần giữ:
1. **Thứ tự:** mọi phần tử `small` ≤ mọi phần tử `large`.
2. **Cân bằng:** chênh lệch kích thước ≤ 1.

→ Median nằm ngay ở đỉnh (1 hoặc 2 đỉnh). Mỗi lần thêm chỉ tốn $O(\log n)$, truy vấn median $O(1)$.

## 3. Template
```cpp
#include <queue>
#include <vector>
using namespace std;

class MedianFinder {
    priority_queue<int> small;                              // Max-Heap - nửa nhỏ
    priority_queue<int, vector<int>, greater<int>> large;  // Min-Heap - nửa lớn
public:
    void addNum(int num) {
        small.push(num);
        // đẩy phần tử lớn nhất của small sang large -> giữ bất biến thứ tự
        large.push(small.top()); small.pop();
        // cân bằng kích thước (giữ small >= large)
        if (large.size() > small.size()) {
            small.push(large.top()); large.pop();
        }
    }
    double findMedian() {
        if (small.size() > large.size()) return small.top();
        return (small.top() + large.top()) / 2.0;
    }
};
```

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Hard | [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | "Trùm" của dạng này |
| Hard | [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/) | Two Heaps + lazy deletion |
| Hard | [502. IPO](https://leetcode.com/problems/ipo/) | Hai heap kết hợp tham lam ([[Scheduling, Greedy]]) |

## 5. Bẫy
- ❌ Quên cân bằng sau khi thêm → một bên phình to, median sai.
- ❌ Quên ép kiểu khi chia trung bình → dùng `/ 2.0` để ra `double`, tránh chia nguyên.
- ✅ Quy ước cố định: cho `small` luôn ≥ `large` về kích thước để median lẻ lấy `small.top()`.
