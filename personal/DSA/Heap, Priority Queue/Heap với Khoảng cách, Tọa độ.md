# Heap + Khoảng cách / Tọa độ

> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
"K điểm gần gốc nhất", "K số gần nhất với x", "phần tử có khoảng cách nhỏ/lớn nhất". Bản chất là [[Top K Elements]] nhưng **tiêu chí so sánh là khoảng cách** (đôi khi tính toán phức tạp).

## 2. Tư duy cốt lõi
- Định nghĩa **hàm khoảng cách** (Euclid bình phương, trị tuyệt đối...) làm khóa so sánh.
- K điểm **gần nhất** → **Max-Heap** size K (loại điểm xa nhất khi vượt K).
- K điểm **xa nhất** → **Min-Heap** size K.
- **Mẹo:** với khoảng cách Euclid, dùng $x^2 + y^2$ (không cần `sqrt`) để so sánh — nhanh và chính xác hơn.

## 3. Template
```cpp
#include <queue>
#include <vector>
#include <array>
using namespace std;

// K điểm gần gốc nhất -> Max-Heap size K (theo khoảng cách)
vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
    // Max-Heap mặc định: phần tử {dist, x, y}, đỉnh là điểm XA nhất
    priority_queue<array<int,3>> heap;
    for (auto& p : points) {
        int x = p[0], y = p[1];
        int dist = x * x + y * y;       // không cần sqrt
        heap.push({dist, x, y});
        if ((int)heap.size() > k)
            heap.pop();                 // loại điểm XA nhất
    }
    vector<vector<int>> res;
    while (!heap.empty()) {
        auto [d, x, y] = heap.top(); heap.pop();
        res.push_back({x, y});
    }
    return res;
}
```

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Medium | [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | Heap theo khoảng cách Euclid |
| Medium | [658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/) | Khoảng cách |x - target| (so thêm binary search) |
| Hard | [1102. Path With Maximum Minimum Value](https://leetcode.com/problems/path-with-maximum-minimum-value/) | Heap + tọa độ trên lưới (Dijkstra-like) |

## 5. Bẫy
- ❌ Dùng `sqrt` gây sai số dấu phẩy động khi so sánh khoảng cách bằng nhau.
- ❌ Lưu `{dist, vector}` → khó so sánh; dùng `array<int,3>` dạng `{dist, x, y}` cho gọn.
- ✅ K gần nhất → Max-Heap; K xa nhất → Min-Heap (nhớ tư duy ngược như Dạng A).
