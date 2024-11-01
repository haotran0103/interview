Dưới đây là một số câu hỏi kèm theo cách trả lời với code mẫu đơn giản, tập trung vào giải thích từng bước để dễ hiểu nhất:

---

### 1. **Tìm phần tử duy nhất trong mảng**
   - **Câu hỏi:** Trong một mảng các số nguyên, có một phần tử xuất hiện duy nhất, tất cả phần tử khác xuất hiện đúng hai lần. Tìm phần tử duy nhất đó.
   - **Cách giải:** Sử dụng phép XOR (`^`) để tìm phần tử duy nhất. Vì `a ^ a = 0` và `a ^ 0 = a`, mọi phần tử xuất hiện hai lần sẽ bị loại bỏ khi XOR với chính nó, chỉ còn lại phần tử xuất hiện một lần.

   ```python
   def find_unique(arr):
       unique = 0
       for num in arr:
           unique ^= num
       return unique

   # Ví dụ
   arr = [4, 1, 2, 1, 2]
   print(find_unique(arr))  # Output: 4
   ```

---

### 2. **Kiểm tra tính đối xứng của chuỗi dấu ngoặc**
   - **Câu hỏi:** Cho một chuỗi gồm các dấu ngoặc như `()`, `[]`, `{}`. Kiểm tra xem chuỗi có đối xứng và hợp lệ không.
   - **Cách giải:** Sử dụng ngăn xếp (stack) để lưu các dấu ngoặc mở, và khi gặp dấu ngoặc đóng thì kiểm tra tính đối xứng với dấu ngoặc mở trên cùng của stack.

   ```python
   def is_valid_parentheses(s):
       stack = []
       mapping = {")": "(", "]": "[", "}": "{"}
       for char in s:
           if char in mapping:
               top_element = stack.pop() if stack else '#'
               if mapping[char] != top_element:
                   return False
           else:
               stack.append(char)
       return not stack

   # Ví dụ
   s = "([{}])"
   print(is_valid_parentheses(s))  # Output: True
   ```

---

### 3. **Tìm phần tử giữa của danh sách liên kết**
   - **Câu hỏi:** Cho một danh sách liên kết đơn, hãy tìm phần tử ở giữa danh sách.
   - **Cách giải:** Dùng hai con trỏ, một con trỏ di chuyển một bước và một con trỏ di chuyển hai bước. Khi con trỏ di chuyển hai bước đến cuối danh sách thì con trỏ một bước sẽ ở giữa.

   ```python
   class ListNode:
       def __init__(self, val=0, next=None):
           self.val = val
           self.next = next

   def find_middle(head):
       slow = head
       fast = head
       while fast and fast.next:
           slow = slow.next
           fast = fast.next.next
       return slow.val

   # Ví dụ
   head = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
   print(find_middle(head))  # Output: 3
   ```

---

### 4. **Tìm dãy con có tổng lớn nhất (Maximum Subarray)**
   - **Câu hỏi:** Tìm dãy con liên tiếp có tổng lớn nhất trong một mảng số nguyên.
   - **Cách giải:** Sử dụng thuật toán Kadane để tính tổng lớn nhất của dãy con. Giữ một tổng tạm thời và nếu tổng tạm thời nhỏ hơn 0, đặt lại nó về 0.

   ```python
   def max_subarray(nums):
       max_sum = nums[0]
       current_sum = 0
       for num in nums:
           current_sum += num
           max_sum = max(max_sum, current_sum)
           if current_sum < 0:
               current_sum = 0
       return max_sum

   # Ví dụ
   nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
   print(max_subarray(nums))  # Output: 6
   ```

---

### 5. **Kiểm tra hai chuỗi có phải là hoán vị của nhau không**
   - **Câu hỏi:** Cho hai chuỗi, kiểm tra xem chúng có phải là hoán vị của nhau không.
   - **Cách giải:** Nếu hai chuỗi là hoán vị của nhau, khi sắp xếp chúng, chúng sẽ giống nhau.

   ```python
   def are_permutations(str1, str2):
       return sorted(str1) == sorted(str2)

   # Ví dụ
   str1 = "listen"
   str2 = "silent"
   print(are_permutations(str1, str2))  # Output: True
   ```

---

### 6. **Duyệt cây nhị phân theo thứ tự trước (Pre-order Traversal)**
   - **Câu hỏi:** Cho một cây nhị phân, hãy in các nút theo thứ tự trước.
   - **Cách giải:** Đệ quy lần lượt in giá trị của nút gốc, sau đó đến cây con trái, và cuối cùng là cây con phải.

   ```python
   class TreeNode:
       def __init__(self, val=0, left=None, right=None):
           self.val = val
           self.left = left
           self.right = right

   def pre_order_traversal(root):
       if root:
           print(root.val, end=' ')
           pre_order_traversal(root.left)
           pre_order_traversal(root.right)

   # Ví dụ
   root = TreeNode(1, TreeNode(2), TreeNode(3, TreeNode(4), TreeNode(5)))
   pre_order_traversal(root)  # Output: 1 2 3 4 5
   ```

---

### 7. **Sắp xếp và tìm kiếm phần tử bằng Binary Search**
   - **Câu hỏi:** Tìm phần tử trong mảng đã sắp xếp bằng Binary Search.
   - **Cách giải:** Sử dụng tìm kiếm nhị phân (Binary Search) với độ phức tạp O(log n).

   ```python
   def binary_search(arr, target):
       left, right = 0, len(arr) - 1
       while left <= right:
           mid = (left + right) // 2
           if arr[mid] == target:
               return mid
           elif arr[mid] < target:
               left = mid + 1
           else:
               right = mid - 1
       return -1

   # Ví dụ
   arr = [1, 2, 3, 4, 5, 6, 7]
   target = 5
   print(binary_search(arr, target))  # Output: 4 (vị trí của 5)
   ```

---

### 8. **Kiểm tra cây nhị phân tìm kiếm hợp lệ**
   - **Câu hỏi:** Kiểm tra xem một cây có phải là cây nhị phân tìm kiếm (BST) không.
   - **Cách giải:** Đệ quy kiểm tra giá trị của từng nút để đảm bảo các giá trị bên trái nhỏ hơn nút hiện tại và các giá trị bên phải lớn hơn.

   ```python
   def is_valid_bst(root, low=float('-inf'), high=float('inf')):
       if not root:
           return True
       if not (low < root.val < high):
           return False
       return (is_valid_bst(root.left, low, root.val) and
               is_valid_bst(root.right, root.val, high))

   # Ví dụ
   root = TreeNode(2, TreeNode(1), TreeNode(3))
   print(is_valid_bst(root))  # Output: True
   ```

---

Các câu trả lời này sẽ giúp bạn giải các câu hỏi phổ biến một cách đơn giản và hiệu quả, phù hợp với yêu cầu của buổi phỏng vấn.
