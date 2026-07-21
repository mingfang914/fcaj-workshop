---
title: "Kiểm thử và xác minh"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

# Kiểm thử Smart Image Platform

Các bước dưới đây dùng deployment CDK `staging`. Nếu tài nguyên được tạo thủ công, thay tên theo tài nguyên tương ứng.

## 1. Xác thực người dùng

1. Mở Amplify branch URL hoặc custom domain trỏ đến Amplify.
2. Đăng ký bằng email, tên và mật khẩu đáp ứng password policy.
3. Nhập mã xác nhận nhận qua email.
4. Đăng nhập và kiểm tra ID token chứa thông tin User Pool; tài khoản admin cần được thêm vào group `admin`.

![Màn hình sau khi xác nhận đăng ký](/images/5-Workshop/5.7-Testing-Validation/react_app_login.png)

## 2. Upload và xử lý ảnh

1. Chọn JPEG hoặc PNG hợp lệ trên giao diện Upload Images.
2. Kiểm tra request lấy presigned URL thành công và trình duyệt PUT trực tiếp object vào raw bucket.
3. Theo dõi trạng thái ảnh trong giao diện; quá trình xử lý là bất đồng bộ nên cần polling cho đến trạng thái cuối.
4. Trong processed bucket, kiểm tra các prefix `resized/` và `thumbnails/` dưới `users/<user-id>/`.

![Các thư mục output trong processed bucket staging](/images/5-Workshop/5.7-Testing-Validation/s3_processed_objects.png)

## 3. Metadata và Rekognition

Mở bảng `SmartImage-Images-staging`, dùng **Explore items** và kiểm tra item ảnh:

- `PK`/`SK` đúng access pattern.
- `status` phản ánh tiến trình xử lý.
- `thumbnailKey`/`resizedKey` trỏ đến processed bucket.
- `aiTags`, moderation labels/status và EXIF được thêm khi các bước tương ứng hoàn tất.

![Kiểm tra aiTags trên item DynamoDB](/images/5-Workshop/5.7-Testing-Validation/dynamodb_item_tags.png)

Ảnh chỉ dùng để quan sát; không chỉnh sửa `aiTags` thủ công trên màn hình Edit item.

## 4. Frontend gallery

Mở My Gallery, chọn ảnh đã xử lý và kiểm tra ảnh preview, trạng thái, metadata và nhãn AI. Community Gallery gọi `GET /v1/images/public`; My Gallery gọi route được bảo vệ bởi Cognito.

![Ảnh ở trạng thái COMPLETED và các nhãn Rekognition](/images/5-Workshop/5.7-Testing-Validation/react_app_dashboard.png)

## 5. Logs, metrics và cảnh báo

Kiểm tra:

- Lambda log groups có tên tương ứng `SmartImage-ApiHandler-staging`, `SmartImage-ImageProcessor-staging`, `SmartImage-AiAnalyzer-staging`.
- API access log group `/aws/apigateway/SmartImage-staging`.
- Dashboard `SmartImage-staging-Operations`.
- SNS topic `SmartImage-Alarms-staging`; email subscription phải được xác nhận trước khi nhận cảnh báo.
- SQS queues `SmartImage-ImageProcessorDlq-staging` và `SmartImage-AiAnalyzerDlq-staging`.

Alarm Lambda Errors dùng `Sum` trong 5 phút, threshold 5 và toán tử **GreaterThanThreshold**; cần ít nhất 6 lỗi trong một chu kỳ để chuyển sang ALARM.

## 6. Kiểm thử lỗi

- Dùng file có extension không được hỗ trợ để kiểm tra frontend validation; file bị chặn trước khi upload nên không tạo Lambda error.
- Để kiểm tra backend validation, có thể upload một object có extension ảnh nhưng nội dung hỏng trực tiếp vào raw bucket `staging`, sau đó kiểm tra log và trạng thái item.
- `ImageProcessor` và `AiAnalyzer` hiện bắt lỗi mà không ném lại trong một số nhánh. Khi invocation vẫn được Lambda xem là thành công, metric `Errors`, retry và DLQ sẽ không hoạt động như một lỗi chưa xử lý.
- Chỉ ghi nhận SNS/DLQ là đã kiểm thử thành công sau khi error handling được sửa để trả lỗi hoặc khi dùng một bài test alarm độc lập có kiểm soát.

> Không tạo lỗi lặp lại trên production. Xóa object/item thử nghiệm sau khi kiểm tra.
