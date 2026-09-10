# 🚀 LeetCode Blind 75 Solutions & Optimization Notes

Lưu trữ lời giải và ghi chú tối ưu hóa thuật toán cho **Blind 75 LeetCode Questions**.  
Mỗi bài tập được phân loại theo Dạng bài (Topic), kèm theo phân tích độ phức tạp thời gian/không gian (**Time & Space Complexity**) từ phương pháp vét cạn (Brute Force) đến tối ưu (Optimal).

---

## 📊 Tiến độ hoàn thành (Progress Tracker)

- **Tổng cộng**: `20 / 75` bài
  - 🟢 **Easy**: `18 / 19`
  - 🟡 **Medium**: `2 / 49`
  - 🔴 **Hard**: `0 / 7`

```
[████████░░░░░░░░░░░░░░░░░░░░░░] 26.7% Completed
```

---

## 📑 Danh mục bài tập (Blind 75 Checklist)

### 1. Array (Mảng) — `01-array/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [x] | #0001 | [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | Hash Map / One-pass | [Solution](01-array/0001_two_sum.md) | Lưu complement `target - num` |
| [x] | #0121 | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | Easy | Sliding Window / Greedy | [Solution](01-array/0121_best_time_to_buy_and_sell_stock.md) | Theo dõi `min_price` & `max_profit` |
| [x] | #0217 | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) | Easy | Hash Set | [Solution](01-array/0217_contains_duplicate.md) | `set()` / Early Exit |
| [ ] | #0238 | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | Medium | Prefix & Suffix Products | [Solution](01-array/0238_product_of_array_except_self.md) | Không dùng phép chia |
| [ ] | #0053 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | Medium | Kadane's Algorithm / DP | [Solution](01-array/0053_maximum_subarray.md) | |
| [ ] | #0152 | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | Medium | Dynamic Programming / Min-Max | [Solution](01-array/0152_maximum_product_subarray.md) | Chú ý số âm |
| [ ] | #0153 | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | Medium | Binary Search | [Solution](01-array/0153_find_minimum_in_rotated_sorted_array.md) | |
| [ ] | #0033 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | Medium | Modified Binary Search | [Solution](01-array/0033_search_in_rotated_sorted_array.md) | Xác định nửa được sort |
| [ ] | #0015 | [3Sum](https://leetcode.com/problems/3sum/) | Medium | Sorting + Two Pointers | [Solution](01-array/0015_3sum.md) | Xử lý trùng lặp (duplicate) |
| [ ] | #0011 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Medium | Two Pointers (Greedy shrink) | [Solution](01-array/0011_container_with_most_water.md) | Di chuyển cột thấp hơn |

---

### 2. Binary / Bit Manipulation (Thao tác bit) — `02-binary/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0371 | [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/) | Medium | Bitwise XOR & AND (Carry) | [Solution](02-binary/0371_sum_of_two_integers.md) | Không dùng toán tử `+`, `-` |
| [x] | #0191 | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | Easy | Brian Kernighan (`n & (n - 1)`) | [Solution](02-binary/0191_number_of_1_bits.md) | Xóa bit 1 thấp nhất |
| [x] | #0338 | [Counting Bits](https://leetcode.com/problems/counting-bits/) | Easy | DP + Bit Manipulation | [Solution](02-binary/0338_counting_bits.md) | `ans[i] = ans[i & (i - 1)] + 1` |
| [x] | #0268 | [Missing Number](https://leetcode.com/problems/missing-number/) | Easy | XOR / Gauss Sum Formula | [Solution](02-binary/0268_missing_number.md) | Tổng Gauss: $n(n+1)/2 - \sum$ |
| [x] | #0190 | [Reverse Bits](https://leetcode.com/problems/reverse-bits/) | Easy | Bit Shifting | [Solution](02-binary/0190_reverse_bits.md) | Băng chuyền `res << 1` 32 lần |

---

### 3. Dynamic Programming (Quy hoạch động) — `03-dynamic-programming/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [x] | #0070 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Easy | 1D DP / Fibonacci | [Solution](03-dynamic-programming/0070_climbing_stairs.md) | Fibonacci: 2 biến $O(1)$ Space |
| [ ] | #0322 | [Coin Change](https://leetcode.com/problems/coin-change/) | Medium | Unbounded Knapsack / Bottom-up DP | [Solution](03-dynamic-programming/0322_coin_change.md) | |
| [ ] | #0300 | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | Medium | DP $O(N^2)$ / Binary Search $O(N \log N)$ | [Solution](03-dynamic-programming/0300_longest_increasing_subsequence.md) | Patient Sorting |
| [ ] | #1143 | [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | Medium | 2D Grid DP | [Solution](03-dynamic-programming/1143_longest_common_subsequence.md) | |
| [ ] | #0139 | [Word Break](https://leetcode.com/problems/word-break/) | Medium | 1D DP / Hash Set | [Solution](03-dynamic-programming/0139_word_break.md) | |
| [ ] | #0377 | [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) | Medium | 1D DP (Permutations count) | [Solution](03-dynamic-programming/0377_combination_sum_iv.md) | |
| [ ] | #0198 | [House Robber](https://leetcode.com/problems/house-robber/) | Medium | 1D DP (Include / Exclude) | [Solution](03-dynamic-programming/0198_house_robber.md) | |
| [ ] | #0213 | [House Robber II](https://leetcode.com/problems/house-robber-ii/) | Medium | DP on Circular Array | [Solution](03-dynamic-programming/0213_house_robber_ii.md) | Chia 2 TH: bỏ nhà 1 hoặc nhà n |
| [ ] | #0091 | [Decode Ways](https://leetcode.com/problems/decode-ways/) | Medium | 1D DP / State Transition | [Solution](03-dynamic-programming/0091_decode_ways.md) | Cẩn thận số `0` |
| [ ] | #0062 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | Medium | 2D Grid DP / Combinatorics | [Solution](03-dynamic-programming/0062_unique_paths.md) | |
| [ ] | #0055 | [Jump Game](https://leetcode.com/problems/jump-game/) | Medium | Greedy / Max Reachable Index | [Solution](03-dynamic-programming/0055_jump_game.md) | |

---

### 4. Graph (Đồ thị) — `04-graph/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0133 | [Clone Graph](https://leetcode.com/problems/clone-graph/) | Medium | BFS / DFS + Hash Map (Visited) | [Solution](04-graph/0133_clone_graph.md) | |
| [ ] | #0207 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | Medium | Topological Sort (Kahn's / DFS cycle) | [Solution](04-graph/0207_course_schedule.md) | Detect Cycle trong directed graph |
| [ ] | #0417 | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) | Medium | Multi-source DFS/BFS từ viền | [Solution](04-graph/0417_pacific_atlantic_water_flow.md) | Đi ngược từ biển vào lục địa |
| [ ] | #0200 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | Medium | Grid DFS/BFS / Union-Find | [Solution](04-graph/0200_number_of_islands.md) | |
| [ ] | #0128 | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | Medium | Hash Set / Sequence Start check | [Solution](04-graph/0128_longest_consecutive_sequence.md) | Đạt $O(N)$ không cần sort |
| [ ] | #0269 | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) 👑 | Hard | Topological Sort (DAG) | [Solution](04-graph/0269_alien_dictionary.md) | LeetCode Premium |
| [ ] | #0261 | [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/) 👑 | Medium | Union-Find / DFS (Cycle + Component) | [Solution](04-graph/0261_graph_valid_tree.md) | LeetCode Premium |
| [ ] | #0323 | [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) 👑 | Medium | Union-Find (Disjoint Set) / BFS | [Solution](04-graph/0323_number_of_connected_components_in_an_undirected_graph.md) | LeetCode Premium |

---

### 5. Interval (Khoảng) — `05-interval/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0057 | [Insert Interval](https://leetcode.com/problems/insert-interval/) | Medium | Linear Scan / Greedy Merge | [Solution](05-interval/0057_insert_interval.md) | |
| [ ] | #0056 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Sorting by Start Time + Greedy | [Solution](05-interval/0056_merge_intervals.md) | |
| [ ] | #0435 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | Medium | Greedy (Sort by End Time) | [Solution](05-interval/0435_non_overlapping_intervals.md) | Interval Scheduling |
| [ ] | #0252 | [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) 👑 | Easy | Sorting + Overlap Check | [Solution](05-interval/0252_meeting_rooms.md) | LeetCode Premium |
| [ ] | #0253 | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) 👑 | Medium | Min-Heap / Two Pointers Chronological | [Solution](05-interval/0253_meeting_rooms_ii.md) | LeetCode Premium |

---

### 6. Linked List (Danh sách liên kết) — `06-linked-list/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [x] | #0206 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | Easy | Iterative (3 Pointers) / Recursive | [Solution](06-linked-list/0206_reverse_linked_list.md) | 3 con trỏ: Lưu sau -> Bẻ ngược |
| [x] | #0141 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | Easy | Floyd's Tortoise and Hare (Slow/Fast) | [Solution](06-linked-list/0141_linked_list_cycle.md) | Rùa & Thỏ: $O(1)$ Space |
| [x] | #0021 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | Easy | Dummy Node + Two Pointers | [Solution](06-linked-list/0021_merge_two_sorted_lists.md) | Dummy Node + Nối đuôi |
| [ ] | #0023 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | Min-Heap / Divide & Conquer | [Solution](06-linked-list/0023_merge_k_sorted_lists.md) | |
| [ ] | #0019 | [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | Medium | Fast/Slow Pointers (Khoảng cách N) | [Solution](06-linked-list/0019_remove_nth_node_from_end_of_list.md) | Dùng Dummy Head |
| [ ] | #0143 | [Reorder List](https://leetcode.com/problems/reorder-list/) | Medium | Find Middle + Reverse + Merge | [Solution](06-linked-list/0143_reorder_list.md) | Kết hợp 3 kỹ thuật |

---

### 7. Matrix (Ma trận) — `07-matrix/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0073 | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | Medium | In-place Flagging (Hàng/Cột 0) | [Solution](07-matrix/0073_set_matrix_zeroes.md) | Đạt $O(1)$ Space |
| [ ] | #0054 | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | Medium | Boundary Simulation (4 con trỏ) | [Solution](07-matrix/0054_spiral_matrix.md) | |
| [ ] | #0048 | [Rotate Image](https://leetcode.com/problems/rotate-image/) | Medium | Transpose + Reverse Rows | [Solution](07-matrix/0048_rotate_image.md) | Xoay 90 độ In-place |
| [ ] | #0079 | [Word Search](https://leetcode.com/problems/word-search/) | Medium | Grid Backtracking / DFS | [Solution](07-matrix/0079_word_search.md) | Đánh dấu ô đã đi |

---

### 8. String (Chuỗi ký tự) — `08-string/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0003 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium | Sliding Window + Hash Map | [Solution](08-string/0003_longest_substring_without_repeating_characters.md) | |
| [ ] | #0424 | [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/) | Medium | Sliding Window + Max Frequency | [Solution](08-string/0424_longest_repeating_character_replacement.md) | Window len - maxFreq <= k |
| [ ] | #0076 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | Hard | Sliding Window + Char Count Match | [Solution](08-string/0076_minimum_window_substring.md) | Bài mẫu mực Sliding Window |
| [x] | #0242 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | Easy | Frequency Array / Hash Map | [Solution](08-string/0242_valid_anagram.md) | Đếm tần suất ký tự |
| [ ] | #0049 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | Categorize by Sorted String / Count | [Solution](08-string/0049_group_anagrams.md) | |
| [x] | #0020 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | Easy | Stack | [Solution](08-string/0020_valid_parentheses.md) | Stack LIFO + Hash Map matching |
| [x] | #0125 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | Easy | Two Pointers (Left & Right) | [Solution](08-string/0125_valid_palindrome.md) | In-place $O(1)$ Space, bỏ qua ký tự đặc biệt |
| [ ] | #0005 | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) | Medium | Expand Around Center / DP | [Solution](08-string/0005_longest_palindromic_substring.md) | $O(N^2)$ Time, $O(1)$ Space |
| [ ] | #0647 | [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/) | Medium | Expand Around Center | [Solution](08-string/0647_palindromic_substrings.md) | |
| [ ] | #0271 | [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/) 👑 | Medium | Length Prefix Delimiter (VD: `4#lint`) | [Solution](08-string/0271_encode_and_decode_strings.md) | LeetCode Premium |

---

### 9. Tree & BST (Cây & Cây tìm kiếm nhị phân) — `09-tree/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [x] | #0104 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | Easy | DFS / BFS Level Order | [Solution](09-tree/0104_maximum_depth_of_binary_tree.md) | DFS `max(l, r) + 1` / BFS deque |
| [x] | #0100 | [Same Tree](https://leetcode.com/problems/same-tree/) | Easy | Recursive DFS | [Solution](09-tree/0100_same_tree.md) | So sánh cấu trúc và p.val == q.val |
| [x] | #0226 | [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) | Easy | Recursive DFS / BFS | [Solution](09-tree/0226_invert_binary_tree.md) | Đổi chỗ con trái - phải |
| [ ] | #0124 | [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | Hard | Postorder DFS + Global Max | [Solution](09-tree/0124_binary_tree_maximum_path_sum.md) | Bỏ qua nhánh có tổng âm |
| [x] | #0102 | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | Medium | BFS (Queue với kích thước từng tầng) | [Solution](09-tree/0102_binary_tree_level_order_traversal.md) | |
| [ ] | #0297 | [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | Hard | Preorder DFS / BFS String Encoding | [Solution](09-tree/0297_serialize_and_deserialize_binary_tree.md) | |
| [x] | #0572 | [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/) | Easy | Recursive IsSameTree traversal | [Solution](09-tree/0572_subtree_of_another_tree.md) | |
| [ ] | #0105 | [Construct Binary Tree from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | Medium | Preorder Root + Inorder Partition | [Solution](09-tree/0105_construct_binary_tree_from_preorder_and_inorder_traversal.md) | Dùng Hash Map cho Inorder |
| [x] | #0098 | [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) | Medium | DFS với Min/Max Range / Inorder | [Solution](09-tree/0098_validate_binary_search_tree.md) | Inorder của BST luôn tăng dần |
| [ ] | #0230 | [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) | Medium | Inorder Traversal (Iterative / Stack) | [Solution](09-tree/0230_kth_smallest_element_in_a_bst.md) | |
| [ ] | #0235 | [Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | Medium | BST Properties (Chia nhánh) | [Solution](09-tree/0235_lowest_common_ancestor_of_a_binary_search_tree.md) | $O(\log N)$ |
| [ ] | #0208 | [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | Medium | Trie Node (Children map + isEnd) | [Solution](09-tree/0208_implement_trie_prefix_tree.md) | Cấu trúc dữ liệu Trie cơ bản |
| [ ] | #0211 | [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | Medium | Trie + DFS Backtracking (cho dấu `.`) | [Solution](09-tree/0211_design_add_and_search_words_data_structure.md) | |
| [ ] | #0212 | [Word Search II](https://leetcode.com/problems/word-search-ii/) | Hard | Trie + 2D Grid DFS Backtracking | [Solution](09-tree/0212_word_search_ii.md) | Tối ưu với Trie pruning |

---

### 10. Heap / Priority Queue — `10-heap/`
| Status | ID | Tên bài | Difficulty | Pattern / Key Technique | Solution | Ghi chú |
| :---: | :---: | :--- | :---: | :--- | :---: | :--- |
| [ ] | #0023 | [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | Hard | Min-Heap / Divide and Conquer | [Solution](10-heap/0023_merge_k_sorted_lists.md) | |
| [ ] | #0347 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium | Min-Heap / Bucket Sort | [Solution](10-heap/0347_top_k_frequent_elements.md) | Bucket Sort đạt $O(N)$ |
| [ ] | #0295 | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | Hard | Two Heaps (Max-Heap + Min-Heap) | [Solution](10-heap/0295_find_median_from_data_stream.md) | Cân bằng 2 nửa mảng |

---

## 🛠️ Hướng dẫn cách ghi chép & Tối ưu bài giải

Để đạt hiệu quả phỏng vấn cao nhất, mỗi khi giải một bài, bạn nên tuân thủ quy trình 3 bước:

1. **Hiểu đề & Nghĩ Brute Force trước**: Viết ra cách đơn giản nhất (dù là $O(N^2)$ hay $O(2^N)$), tính Time/Space Complexity.
2. **Tìm điểm nghẽn (Bottleneck)**: Nhận diện xem ta đang lặp lại phép tính nào? Có thể dùng cấu trúc dữ liệu nào (Hash Map, Heap, Two Pointers, Monotonic Stack, Trie, DP) để giảm thời gian không?
3. **Viết lời giải tối ưu (Optimal Solution)**: Ghi chú lại bài học cốt lõi (Key Takeaways) để áp dụng cho các bài tương tự.
