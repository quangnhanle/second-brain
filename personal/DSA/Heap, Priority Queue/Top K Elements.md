> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
Tìm **K phần tử lớn nhất / nhỏ nhất / hay xuất hiện nhất**. Từ khóa: *"Top K", "K most frequent", "K largest/smallest"*.

## 2. Tư duy cốt lõi — Mẹo "ngược"
- Muốn tìm **K phần tử LỚN nhất** → dùng **Min-Heap** kích thước K.
- Muốn tìm **K phần tử NHỎ nhất** → dùng **Max-Heap** kích thước K.

Lý do: heap luôn giữ đúng K "ứng viên", và **đỉnh heap chính là ngưỡng cửa**. Phần tử mới mà "thua" đỉnh thì loại ngay → heap không bao giờ phình quá K.

- **Độ phức tạp:** $O(n \log k)$ — tốt hơn sort $O(n \log n)$ khi $k \ll n$.

## 3. Template
```cpp
#include <queue>
#include <vector>
#include <unordered_map>
using namespace std;

// K phần tử LỚN nhất -> Min-Heap size K
vector<int> topKLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> heap;   // Min-Heap
    for (int num : nums) {
        heap.push(num);
        if ((int)heap.size() > k)
            heap.pop();           // loại nhỏ nhất, giữ K phần tử lớn nhất
    }
    vector<int> res;              // heap.top() = phần tử thứ K lớn nhất
    while (!heap.empty()) { res.push_back(heap.top()); heap.pop(); }
    return res;
}

// Top K theo tần suất (kết hợp Frequency Map)
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> count;
    for (int num : nums) count[num]++;
    // Min-Heap theo (tần suất, giá trị), giữ K phần tử tần suất cao nhất
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> heap;
    for (auto& [val, cnt] : count) {
        heap.push({cnt, val});
        if ((int)heap.size() > k) heap.pop();
    }
    vector<int> res;
    while (!heap.empty()) { res.push_back(heap.top().second); heap.pop(); }
    return res;
}
```

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Easy | [1086. High Five](https://leetcode.com/problems/high-five/) | Top K + HashMap |
| Medium | [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Heap + Frequency Map — rất hay hỏi |
| Medium | [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/) | Custom comparator (tie-break chữ cái) |
| Medium | [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | Cũng là Top K (xem thêm [[Heap với Khoảng cách, Tọa độ]]) |

## 5. Bẫy
- ❌ Tìm K lớn nhất mà lại dùng Max-Heap rồi pop K lần → vẫn đúng nhưng $O(n + k\log n)$, không tối ưu bộ nhớ với stream.
- ✅ Khi chỉ cần lấy 1 lần và k gần n → cân nhắc `sort()` cho đơn giản.
