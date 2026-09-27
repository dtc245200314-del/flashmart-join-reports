# Nhật ký trao đổi với AI về JOIN

## 1. `JOIN` mặc định hoạt động thế nào?

**Prompt:** Trong MySQL, từ khóa `JOIN` không ghi rõ `LEFT` hay `RIGHT` hoạt động như thế nào? Nó giữ hay bỏ bản ghi không khớp?

**Trả lời tóm tắt:** `JOIN` mặc định là `INNER JOIN`. Chỉ các cặp dòng thỏa điều kiện `ON` được giữ lại. Vì vậy khách hàng chưa có đơn và sản phẩm chưa xuất hiện trong `Orders` đều biến mất khỏi kết quả.

## 2. Vì sao dùng `COUNT(o.order_id)`?

**Prompt:** Khi dùng `LEFT JOIN` để đếm đơn hàng, khác biệt giữa `COUNT(*)` và `COUNT(o.order_id)` là gì?

**Trả lời tóm tắt:** `COUNT(*)` đếm cả dòng đệm mà phép nối ngoài tạo cho bản ghi không khớp. `COUNT(o.order_id)` bỏ qua `NULL`, nên khách hàng chưa có đơn nhận giá trị 0. Chọn khóa chính của bảng `Orders` còn bảo đảm mỗi giá trị được đếm là một đơn thực.

## 3. Anti-Join và `NOT IN`

**Prompt:** `LEFT JOIN ... WHERE o.order_id IS NULL` khác gì `NOT IN` khi tìm sản phẩm chưa được bán?

**Trả lời tóm tắt:** Anti-join diễn đạt trực tiếp việc giữ bản ghi bên trái không có đối tác. `NOT IN` có thể trả về kết quả ngoài ý muốn nếu tập con chứa `NULL`. MySQL thường có thể tối ưu anti-join hoặc `NOT EXISTS` hiệu quả khi cột nối có chỉ mục. Với dữ liệu có khả năng chứa `NULL`, `NOT EXISTS` hoặc anti-join an toàn và rõ ràng hơn.

## 4. MySQL tối ưu JOIN bằng Nested-Loop thế nào?

**Prompt:** Hãy giải thích cách MySQL Optimizer dùng Nested-Loop Join và vai trò của chỉ mục trên cột nối.

**Trả lời tóm tắt:** Optimizer chọn thứ tự đọc bảng dựa trên chi phí ước lượng. Với nested loop, mỗi dòng từ bảng ngoài được dùng để tìm dòng phù hợp ở bảng trong. Chỉ mục trên `Orders.customer_id` và `Orders.product_id` giúp biến việc tìm kiếm lặp lại thành tra cứu nhanh, thay vì quét toàn bộ `Orders` cho từng dòng. Có thể dùng `EXPLAIN` để xem thứ tự bảng, kiểu truy cập và chỉ mục được chọn.

## 5. `RIGHT JOIN` và `CROSS JOIN`

**Prompt:** Nếu giữ nguyên thứ tự `Customers c RIGHT JOIN Orders o`, điều gì xảy ra? Khi nào JOIN vô tình thành tích Descartes?

**Trả lời tóm tắt:** `RIGHT JOIN` sẽ giữ toàn bộ `Orders`, không giữ toàn bộ `Customers`, nên Charlie vẫn mất. `CROSS JOIN` tạo mọi tổ hợp giữa hai bảng. Một phép nối có thể gây hiệu ứng tương tự khi thiếu hoặc viết sai điều kiện `ON`, làm số dòng tăng theo tích kích thước hai bảng.
