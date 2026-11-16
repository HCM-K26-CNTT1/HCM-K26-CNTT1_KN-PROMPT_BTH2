# BÀI THỰC HÀNH 2: PHÂN TÍCH CẢM XÚC VÀ GẮN TAG PHẢN HỒI KHÁCH HÀNG

## PHẦN 1. XÂY DỰNG PROMPT CREATE

```text
[C - Context]
Bạn là chuyên gia phân tích dữ liệu thị trường và phản hồi khách hàng.

[R - Request]
Phân loại phản hồi theo 3 thuộc tính:
- Sentiment: Positive, Negative, Neutral.
- Topics: Product, Delivery, Service, System.
- Score: Điểm hài lòng từ 1 đến 5.

[E - Explanation]
Kết quả dùng cho Dashboard thống kê mức độ hài lòng khách hàng.

[A - Action]
- Đọc và phân tích phản hồi.
- Xác định cảm xúc chính.
- Gắn tất cả tag chủ đề phù hợp.
- Chấm điểm từ 1 đến 5.
- Tóm tắt dưới 15 từ.
- Nếu phản hồi trống hoặc không đọc được, trả về Invalid.

[T - Tone]
Chính xác, khách quan, không suy đoán.

[E - Extras: Few-shot]

Ví dụ 1:
Input: "Sản phẩm rất tốt, giao hàng nhanh, nhân viên nhiệt tình."

Output:
{
  "sentiment": "Positive",
  "topics": ["Product", "Delivery", "Service"],
  "score": 5,
  "summary": "Sản phẩm tốt, giao nhanh, nhân viên nhiệt tình"
}

Ví dụ 2:
Input: "Giao hàng nhanh nhưng sản phẩm bị móp méo."

Output:
{
  "sentiment": "Negative",
  "topics": ["Product", "Delivery"],
  "score": 2,
  "summary": "Giao hàng nhanh nhưng sản phẩm bị móp méo"
}

Ví dụ 3:
Input: "Sản phẩm đúng mô tả, chưa có đánh giá thêm."

Output:
{
  "sentiment": "Neutral",
  "topics": ["Product"],
  "score": 3,
  "summary": "Sản phẩm đúng mô tả, chưa có đánh giá thêm"
}

Quy tắc:
- Có thể gắn nhiều topics cho một phản hồi.
- Score từ 1 đến 5.
- Summary dưới 15 từ.
- Không tự thêm thông tin.
- Chỉ trả về JSON thuần túy.
- Nếu phản hồi trống, không đọc được hoặc quá ngắn, không đủ thông tin để xác định cảm xúc và chủ đề (ví dụ: "Ok", "Ừ", "Tốt"), trả về Invalid.
```

## PHẦN 2. KIỂM THỬ PROMPT

### Kiểm thử 1

**Input:**

"Ứng dụng thường xuyên bị lỗi khi thanh toán. Tôi đã thử nhiều lần nhưng vẫn không thành công."

**Output:**

```json
{
  "sentiment": "Negative",
  "topics": ["System"],
  "score": 1,
  "summary": "Ứng dụng thường xuyên lỗi khi thanh toán"
}
```

### Kiểm thử 2

**Input:**

"Sản phẩm có chất lượng tốt, đóng gói cẩn thận nhưng giao hàng hơi chậm."

**Output:**

```json
{
  "sentiment": "Positive",
  "topics": ["Product", "Delivery"],
  "score": 4,
  "summary": "Sản phẩm tốt, đóng gói cẩn thận nhưng giao hàng chậm"
}
```

### Kiểm thử 3

**Input:**

"Tôi đã liên hệ bộ phận hỗ trợ nhiều lần nhưng không nhận được phản hồi."

**Output:**

```json
{
  "sentiment": "Negative",
  "topics": ["Service"],
  "score": 1,
  "summary": "Bộ phận hỗ trợ không phản hồi khách hàng"
}
```

### Kiểm thử 4: Phản hồi quá ngắn, không đủ thông tin

**Input:**

"Ok"

**Output:**

```json
{
  "sentiment": "Invalid",
  "topics": ["Invalid"],
  "score": null,
  "summary": "Invalid feedback"
}
```

## PHẦN 3. XỬ LÝ TRƯỜNG HỢP LỖI

Nếu phản hồi trống hoặc chứa ký tự không thể đọc được:

```json
{
  "sentiment": "Invalid",
  "topics": ["Invalid"],
  "score": null,
  "summary": "Invalid feedback"
}
```
