## Các lỗi syntax:
1. Trong file customers.php 
- if ($customer['active') --> if ($customer['active'])
2. Trong file reports.php 
- $reportRows =[] --> $reportRows =[];
3. Trong file settings.php 
- 'language' => 'en', 
;
--> 'language' => 'en'
];

## Các lỗi logic:
1. Trong file reports.php
- Gán `$category = $product['name'];` thay vì `$product['category']`.
- Nguyên nhân: Báo cáo theo category, không phải name.
- Sửa: Thay `$category = $product['name'];` thành `$category = $product['category'];`.
- Ảnh hưởng: Nhóm sai (theo tên sản phẩm thay vì category), tổng giá trị shelf sai.

2. Trong file checkout.php
- Tính `$subtotal` dùng `=` thay vì `+=`, chỉ lấy giá trị của item cuối trong cart.
- Nguyên nhân: Không cộng dồn subtotal.
- Sửa: Thay `$subtotal = $products[$item['sku']]['price'] * $item['qty'];` thành `$subtotal += $products[$item['sku']]['price'] * $item['qty'];`.
- Ảnh hưởng: Subtotal sai (chỉ bằng item cuối, ví dụ: chỉ 9 thay vì 34), dẫn đến grand total sai.

3. Trong file dashboard.php 
- Low stock kiểm tra `stock < 1`, nhưng ghi chú "Fast-moving campus items should appear here when stock is low" gợi ý ngưỡng cao hơn (ví dụ: <5).
- Nguyên nhân: Ngưỡng quá thấp, chỉ hiện khi hết hàng.
- Sửa: Thay `if ($product['stock'] < 1)` thành `if ($product['stock'] < 5)`.
- Ảnh hưởng: Danh sách low stock ít hơn, không phản ánh "low" đúng.

4. Trong file orders.php 
- Lọc orders với `status === 'completed'` vào `$pendingOnly`, nhưng tiêu đề "Pickup queue" và ghi chú "Only waiting bookstore pickups" yêu cầu pending.
- Nguyên nhân: Logic lọc ngược.
- Sửa: Thay `if ($order['status'] === 'completed')` thành `if ($order['status'] === 'pending')`.
- Ảnh hưởng: Hiển thị completed orders thay vì pending, danh sách sai.

5. Trong file dashboard.php (Dòng 6-10)
- Tính `$totalRevenue` và `$completedOrders` chỉ cho orders có `status === 'pending'`, nhưng ghi chú yêu cầu phản ánh "bookstore orders across the current queue" và "Packed orders".
- Nguyên nhân: "Packed" ngụ ý completed, không phải pending.
- Sửa: Thay `if ($order['status'] === 'pending')` thành `if ($order['status'] === 'completed')`.
- Ảnh hưởng: Revenue và số packed orders sai (chỉ tính pending thay vì completed).
