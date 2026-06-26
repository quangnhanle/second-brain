## 1. Bản chất: Heap là gì và giải quyết vấn đề gì?

**Heap** là một cấu trúc dữ liệu dạng cây nhị phân hoàn chỉnh, được tối ưu cho **một việc duy nhất**: lấy ra phần tử **lớn nhất** (Max-Heap) hoặc **nhỏ nhất** (Min-Heap) một cách cực nhanh.

**Priority Queue (Hàng đợi ưu tiên)** là khái niệm trừu tượng (interface): "hàng đợi mà phần tử có ưu tiên cao nhất luôn ra trước". Heap chính là cách **cài đặt** hiệu quả nhất của Priority Queue.

> **Tư duy cốt lõi:** Khi nào bạn thấy bài toán có các từ khóa **"Top K", "Kth largest/smallest", "Closest", "Most frequent", "Median", "Schedule/ưu tiên"** → gần như chắc chắn là Heap.

| **Thao tác**          | **Độ phức tạp** | **Ý nghĩa**                  |
| --------------------- | --------------- | ---------------------------- |
| Push (thêm phần tử)   | $O(\log n)$     | Thêm và đẩy lên đúng vị trí  |
| Pop (lấy đỉnh)        | $O(\log n)$     | Lấy min/max rồi cân bằng lại |
| Peek (xem đỉnh)       | $O(1)$          | Nhìn min/max mà không lấy ra |
| Heapify (xây từ mảng) | $O(n)$          | Biến mảng bất kỳ thành heap  |

**Lưu ý quan trọng:** Heap **KHÔNG** phải để sắp xếp toàn bộ. Nếu cần toàn bộ thứ tự → dùng `sort` ($O(n \log n)$). Heap mạnh khi bạn chỉ cần **một phần** (top K) hoặc cần **truy cập liên tục** vào min/max khi dữ liệu thay đổi.

---

## 2. Xây dựng Heap & các phương thức chính

### 2.1. Heap được lưu thế nào? (Mảng ngầm)
Heap là **cây nhị phân hoàn chỉnh** nên không cần con trỏ — nó được nhúng gọn vào **một mảng** theo thứ tự duyệt theo tầng. Với node ở chỉ số `i` (0-based):

| Quan hệ          | Công thức     |
| ---------------- | ------------- |
| Cha (parent)     | `(i - 1) / 2` |
| Con trái (left)  | `2*i + 1`     |
| Con phải (right) | `2*i + 2`     |

→ Đây là lý do heap rất nhanh và tiết kiệm bộ nhớ: mọi thao tác chỉ là nhảy chỉ số trên mảng.

### 2.2. Max-Heap vs Min-Heap (Tính bất biến — Heap Property)
- **Max-Heap:** mọi node cha **≥** con → **giá trị lớn nhất nằm ở gốc** (`top`). C++ `priority_queue<int>` mặc định là loại này.
- **Min-Heap:** mọi node cha **≤** con → **giá trị nhỏ nhất nằm ở gốc**. C++ cần `priority_queue<int, vector<int>, greater<int>>`.

> ⚠️ Heap **chỉ** đảm bảo quan hệ cha–con, **KHÔNG** đảm bảo thứ tự giữa các anh em (siblings) hay toàn cục. Vì vậy heap **không phải** cấu trúc đã-sắp-xếp.

### 2.3. Hai cơ chế lõi: Sift-up & Sift-down
Mọi phương thức đều dựa trên 2 thao tác "kéo node về đúng chỗ":

- **Sift-up (bubble-up):** dùng khi **thêm** node ở cuối mảng. So với cha, nếu vi phạm heap property thì đổi chỗ, lặp lên trên. Tối đa $O(\log n)$ bước (chiều cao cây).
- **Sift-down (bubble-down / heapify-down):** dùng khi **xoá gốc**. Đưa node cuối lên gốc rồi so với con lớn hơn (Max-Heap) / nhỏ hơn (Min-Heap), đổi chỗ, lặp xuống dưới. Tối đa $O(\log n)$.

### 2.4. Hai cách XÂY DỰNG Heap từ mảng có sẵn
| Cách | Ý tưởng | Độ phức tạp |
| --- | --- | --- |
| **Chèn lần lượt** | `push` từng phần tử, mỗi lần sift-up | $O(n \log n)$ |
| **Heapify (bottom-up)** ✅ | Sift-down từ node không-lá cuối cùng (`n/2 - 1`) ngược về gốc | **$O(n)$** |

> Heapify nhanh hơn nhờ phân tích chặt: phần lớn node nằm ở tầng thấp nên quãng đường sift-down ngắn → tổng cộng tuyến tính, **không** phải $O(n\log n)$. Trong C++, khởi tạo `priority_queue` từ iterator dùng heapify $O(n)$:
> ```cpp
> vector<int> arr = {3, 1, 4, 1, 5};
> priority_queue<int> pq(arr.begin(), arr.end());   // O(n)
> // (so với push từng phần tử: O(n log n))
> ```

### 2.5. Các phương thức chính & Độ phức tạp
| Phương thức                 | C++ (`priority_queue`)     | Cơ chế                        | Độ phức tạp   |
| --------------------------- | -------------------------- | ----------------------------- | ------------- |
| Xem đỉnh (peek/top)         | `pq.top()`                 | Đọc phần tử gốc               | $O(1)$        |
| Thêm (push/insert)          | `pq.push(x)`               | Thêm cuối + sift-up           | $O(\log n)$   |
| Lấy đỉnh (pop/extract)      | `pq.pop()`                 | Đưa cuối lên gốc + sift-down  | $O(\log n)$   |
| Kích thước / rỗng           | `pq.size()` / `pq.empty()` | Đọc biến đếm                  | $O(1)$        |
| Xây heap (build/heapify)    | constructor từ iterator    | Sift-down bottom-up           | $O(n)$        |
| Tìm phần tử bất kỳ (search) | — (không hỗ trợ trực tiếp) | Phải quét tuyến tính          | $O(n)$        |
| Xoá/cập nhật phần tử bất kỳ | — (cần cấu trúc phụ)       | Tìm $O(n)$ + sift $O(\log n)$ | $O(n)$        |
| Heap Sort (sắp xếp toàn bộ) | pop hết ra                 | $n$ lần pop                   | $O(n \log n)$ |

> 💡 **Lưu ý thực chiến:** `std::priority_queue` **không cho** duyệt, tìm, hay cập nhật phần tử bên trong (no iterator, no `decrease-key`). Khi bài cần **giảm khoá** (như Dijkstra) hoặc **xoá phần tử tuỳ ý** (Sliding Window Median), người ta dùng **lazy deletion** (đánh dấu rồi bỏ qua khi pop) hoặc đổi sang `std::multiset` / `std::set` (hỗ trợ `erase` $O(\log n)$).

---

## 3. Các DẠNG BÀI (Patterns) cần nhận diện

> Mỗi dạng được tách thành một note riêng (bản chất + template + bài tập + bẫy). Bấm vào link để xem chi tiết.

### Dạng A: [[Top K Elements]]
Tìm K phần tử lớn nhất / nhỏ nhất / hay xuất hiện nhất.
- **Mẹo tư duy "ngược":** Muốn tìm **K phần tử LỚN nhất** → dùng **Min-Heap** kích thước K. Đỉnh min-heap chính là "ngưỡng cửa".
- **Độ phức tạp:** $O(n \log k)$ — tốt hơn sort $O(n \log n)$ khi $k \ll n$.

### Dạng B: [[Kth Element]]
Trường hợp đặc biệt của Top K, chỉ lấy đúng phần tử thứ K (đỉnh của heap sau khi xử lý xong).

### Dạng C: [[Two Heaps (Median)]]
Tìm **median (trung vị)** của dòng dữ liệu. Một **Max-Heap** giữ nửa nhỏ, một **Min-Heap** giữ nửa lớn → median nằm ngay ở đỉnh.

### Dạng D: [[Merge K Sorted]]
Gộp K danh sách đã sắp xếp: đưa phần tử đầu của mỗi danh sách vào heap, liên tục lấy nhỏ nhất ra và nạp phần tử kế tiếp.

### Dạng E: [[Scheduling, Greedy]]
Bài toán lập lịch, tối ưu tham lam mà mỗi bước cần chọn phần tử "ưu tiên nhất" hiện tại (CPU task, meeting rooms, connect ropes...).

### Dạng F: [[Heap với Khoảng cách, Tọa độ]]
"K điểm gần gốc nhất", "K số gần nhất" — heap theo tiêu chí khoảng cách.

---

## 4. Template "Vàng" với C++ (`priority_queue`)

C++ mặc định cho **Max-Heap** (`priority_queue<int>`). Muốn Min-Heap → dùng `priority_queue<int, vector<int>, greater<int>>`.

```cpp
#include <queue>
#include <vector>
using namespace std;

// --- Khởi tạo & thao tác cơ bản ---
priority_queue<int> maxHeap;                                   // Max-Heap (mặc định)
priority_queue<int, vector<int>, greater<int>> minHeap;       // Min-Heap

maxHeap.push(5);          // Thêm phần tử          - O(log n)
int top = maxHeap.top();  // Xem đỉnh (không lấy)  - O(1)
maxHeap.pop();            // Lấy đỉnh ra           - O(log n)
int sz = maxHeap.size();  // Số phần tử

// Heapify O(n): khởi tạo trực tiếp từ vector
vector<int> arr = {3, 1, 4, 1, 5};
priority_queue<int> heapFromArr(arr.begin(), arr.end());

// --- TEMPLATE Dạng A: K phần tử LỚN nhất (dùng Min-Heap size K) ---
int kthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;   // size K
    for (int num : nums) {
        minHeap.push(num);
        if ((int)minHeap.size() > k)
            minHeap.pop();           // loại phần tử nhỏ nhất -> giữ K phần tử lớn nhất
    }
    return minHeap.top();            // đỉnh = phần tử thứ K lớn nhất
}

// --- TEMPLATE Dạng C: Two Heaps tìm Median ---
class MedianFinder {
    priority_queue<int> small;                                    // Max-Heap - nửa nhỏ
    priority_queue<int, vector<int>, greater<int>> large;         // Min-Heap - nửa lớn
public:
    void addNum(int num) {
        small.push(num);
        // đảm bảo mọi phần tử small <= mọi phần tử large
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

> **Mẹo với cặp/struct:** `priority_queue<pair<int,int>>` so sánh theo phần tử đầu của `pair` trước, rồi tới phần tử sau. Đẩy dạng `{priority, value}` hoặc `{count, item}`. Muốn sắp theo tiêu chí tùy biến → truyền **comparator** riêng (lambda hoặc struct với `operator()`), ví dụ: `priority_queue<T, vector<T>, decltype(cmp)> pq(cmp);`.

---

## 5. Lộ trình bài tập LeetCode (luyện theo thứ tự)

| **Độ khó** | **Tên bài (LeetCode)** | **Dạng** | **Mục tiêu luyện tập** |
| --- | --- | --- | --- |
| **Easy** | [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) | B | Làm quen Min-Heap size K trên dòng dữ liệu |
| **Easy** | [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) | E | Hiểu Max-Heap (mặc định trong C++) |
| **Easy** | [1086. High Five](https://leetcode.com/problems/high-five/) | A | Top K kết hợp HashMap |
| **Medium** | [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | B | Bài "kinh điển" — so sánh Heap vs QuickSelect |
| **Medium** | [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | A | Heap + Frequency Map (rất hay hỏi) |
| **Medium** | [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | F | Heap theo khoảng cách |
| **Medium** | [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/) | A | Custom comparator (tie-break theo chữ cái) |
| **Medium** | [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/) | E | Greedy + Heap lập lịch CPU |
| **Medium** | [355. Design Twitter](https://leetcode.com/problems/design-twitter/) | D | Merge K feeds bằng heap (design) |
| **Medium** | [1167. Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/) | E | Tham lam Huffman-style |
| **Medium** | [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | E | Min-Heap quản lý phòng họp (interval) |
| **Hard** | [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | D | "Trùm" của dạng Merge K |
| **Hard** | [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | C | "Trùm" của dạng Two Heaps |
| **Hard** | [502. IPO](https://leetcode.com/problems/ipo/) | C/E | Hai heap kết hợp tham lam |
| **Hard** | [767. Reorganize String](https://leetcode.com/problems/reorganize-string/) | E | Heap theo tần suất + ràng buộc |
| **Hard** | [218. The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | E | Heap + sweep line (nâng cao) |

**Gợi ý cách luyện:** Mỗi dạng làm 1 bài Easy/Medium để nắm template, rồi nhảy thẳng "trùm cuối" Hard của dạng đó (215 → 347 → 295 → 23). Khi gặp bài lạ, hãy tự hỏi: *"Mình có cần liên tục lấy min/max khi dữ liệu thay đổi không?"* — nếu CÓ thì nghĩ đến Heap.

---

## 6. Bẫy thường gặp & Checklist

- ❌ **Quên C++ mặc định là Max-Heap.** Cần Min-Heap phải dùng `priority_queue<int, vector<int>, greater<int>>` (ngược với Python mặc định Min-Heap).
- ❌ **Dùng heap khi chỉ cần sort một lần.** Nếu không có yếu tố "stream" hay "top K", `sort()` có khi đơn giản và đủ nhanh hơn.
- ❌ **So sánh phần tử phức tạp** (vd 2 điểm cùng khoảng cách, hoặc `ListNode*`) → đẩy `pair`/`array` có index đệm, hoặc truyền comparator riêng.
- ✅ **Top K Largest → Min-Heap size K**, **Top K Smallest → Max-Heap size K** (nhớ tư duy ngược).
- ✅ **Median / cân bằng 2 nửa → Two Heaps.**
- ✅ Xây heap từ mảng có sẵn dùng constructor từ iterator ($O(n)$) thay vì push lần lượt ($O(n \log n)$).

---

## 7. Góc nhìn Architect: Heap trong hệ thống thực tế

Heap/Priority Queue không chỉ là công cụ giải đố — nó là xương sống của nhiều hệ thống:

- **Task Scheduler / Job Queue:** Hệ điều hành và các message queue (như Celery, RabbitMQ với priority) dùng priority queue để quyết định task nào chạy trước.
- **Dijkstra & A\*:** Thuật toán tìm đường ngắn nhất trong bản đồ (Google Maps, định tuyến mạng) dựa hoàn toàn vào Min-Heap để chọn node gần nhất tiếp theo.
- **Event-driven simulation:** Mô phỏng sự kiện theo thời gian — heap giữ sự kiện sắp xảy ra nhất ở đỉnh.
- **Streaming analytics:** Tính "Top K sản phẩm bán chạy", "Trending hashtags", hoặc median latency của API trong thời gian thực — dùng heap để duy trì kết quả khi dữ liệu chảy liên tục.
- **Load balancing:** Chọn server đang rảnh nhất (ít kết nối nhất) → Min-Heap theo số kết nối.

> **Liên kết note:** [[DSA/Stack, Queue, Deque/Stack, Queue, Deque.md]] · [[DSA/Graph (BFS, DFS, Topological Sort)/Graph (BFS, DFS, Topological Sort).md]] (Dijkstra dùng Heap) · [[DSA/Array, String, HashMap/Frequency Map.md]] (kết hợp với Top K)
