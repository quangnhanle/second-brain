# Scheduling / Greedy với Heap

> Quay lại tổng quan: [[DSA/Heap, Priority Queue/Heap, Priority Queue.md]]

## 1. Nhận diện
Bài toán **lập lịch** hoặc **tham lam (greedy)** mà mỗi bước cần chọn phần tử "ưu tiên nhất hiện tại": task có tần suất cao nhất, cây gỗ ngắn nhất, phòng họp trống sớm nhất...

## 2. Tư duy cốt lõi
- Heap cho phép **luôn lấy được phần tử tối ưu cục bộ** trong $O(\log n)$ khi tập ứng viên thay đổi liên tục.
- Mẫu chung: **lấy ra phần tử tốt nhất → xử lý → (có thể) đẩy lại trạng thái mới vào heap**.
- Thường kết hợp với [[DSA/Array, String, HashMap/Frequency Map.md]] (đếm tần suất) hoặc sắp xếp theo thời gian (interval).

## 3. Template
```cpp
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;

// Mẫu "connect sticks" / Huffman: luôn gộp 2 phần tử nhỏ nhất
long long minCostConnect(vector<int>& sticks) {
    priority_queue<int, vector<int>, greater<int>> heap(sticks.begin(), sticks.end());
    long long cost = 0;
    while (heap.size() > 1) {
        int a = heap.top(); heap.pop();
        int b = heap.top(); heap.pop();
        cost += a + b;
        heap.push(a + b);
    }
    return cost;
}

// Mẫu interval (Meeting Rooms II): Min-Heap lưu thời điểm kết thúc sớm nhất
int minMeetingRooms(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());   // sắp theo thời gian bắt đầu
    priority_queue<int, vector<int>, greater<int>> heap;  // các thời điểm kết thúc
    for (auto& it : intervals) {
        int start = it[0], end = it[1];
        if (!heap.empty() && heap.top() <= start)
            heap.pop();          // tái sử dụng phòng vừa trống
        heap.push(end);
    }
    return heap.size();
}
```

## 4. Bài tập LeetCode
| Độ khó | Bài | Ghi chú |
| --- | --- | --- |
| Easy | [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) | Max-Heap (mặc định trong C++) |
| Medium | [1167. Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/) | Tham lam kiểu Huffman |
| Medium | [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | Min-Heap quản lý interval |
| Medium | [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/) | Greedy + Heap theo tần suất CPU |
| Hard | [767. Reorganize String](https://leetcode.com/problems/reorganize-string/) | Heap theo tần suất + ràng buộc kề nhau |
| Hard | [218. The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | Heap + sweep line (nâng cao) |

## 5. Bẫy
- ❌ Quên đẩy lại trạng thái mới (vd cost mới, task còn lại) vào heap sau khi xử lý.
- ❌ Với interval, quên sort theo thời gian bắt đầu trước khi dùng heap.
- ✅ Cần Max-Heap theo tần suất → `priority_queue<pair<int,int>>` mặc định đã là Max-Heap, đẩy `{count, item}`.
