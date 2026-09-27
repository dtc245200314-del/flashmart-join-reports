# FlashMart JOIN Reports

Bài thực hành khắc phục hai báo cáo SQL bị sai do dùng `INNER JOIN` thay cho `LEFT JOIN`.

## Nội dung nộp bài

- `flashmart_submission.zip`: tệp nén hoàn chỉnh để nộp.
- `flashmart_reports.sql`: tạo dữ liệu mẫu và hai truy vấn báo cáo đã sửa.
- `join_analysis.md`: giải thích lựa chọn `COUNT(o.order_id)` thay cho `COUNT(*)`.
- `ai_prompt_log.md`: nhật ký hỏi đáp về JOIN và tối ưu truy vấn.

## Kết quả đối chiếu

| customer_id | name | total_orders |
|---:|---|---:|
| 1 | Alice | 2 |
| 2 | Bob | 1 |
| 3 | Charlie | 0 |

Sản phẩm chưa từng được bán: `103 | Keyboard`.
