---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

# Dọn dẹp tài nguyên

Chỉ xóa tài nguyên của đúng tài khoản, Region và environment. Không chạy lệnh destroy production khi chưa kiểm tra dữ liệu cần giữ.

## A. Tài nguyên do CDK quản lý

Từ thư mục gốc `AWS-Project`, xác nhận danh sách stack của `staging`:

```bash
npm run --workspace=infrastructure cdk -- list -c environment=staging
```

Sau đó chạy:

```bash
npm run --workspace=infrastructure destroy -- -c environment=staging
```

Trong `staging`, hai S3 bucket được cấu hình `autoDeleteObjects` và `DESTROY`; CDK có thể làm rỗng/xóa bucket trong quá trình destroy. Không cần làm rỗng thủ công trước trừ khi deployment đã bị thay đổi hoặc custom resource lỗi.

Sau khi destroy, kiểm tra CloudFormation và các dịch vụ liên quan để xác nhận sáu stack đã được xóa.

## B. Tài nguyên tạo thủ công trên Console

`cdk destroy` không xóa tài nguyên được tạo riêng trên Console. Xóa theo thứ tự để giảm lỗi dependency:

1. Amplify app/branch.
2. WAF association, sau đó Web ACL.
3. API Gateway stage/API và API access log group.
4. S3 Event Notification, DynamoDB event source mapping và Lambda triggers.
5. Lambda functions và các log groups liên quan.
6. Hai SQS DLQ.
7. DynamoDB tables sau khi kiểm tra backup/PITR requirements.
8. Làm rỗng và xóa raw/processed buckets, bao gồm object versions nếu versioning đã bật.
9. Cognito app client, groups và User Pool.
10. CloudWatch dashboard, alarms, SNS subscriptions/topic.
11. IAM roles và custom policies chỉ được tạo cho workshop.

## C. Kiểm tra sau cleanup

- Kiểm tra lại Region `ap-southeast-1` và các Region khác đã sử dụng.
- Tìm resource có prefix/tag `SmartImage` và environment `staging`.
- Kiểm tra CloudFormation stacks ở trạng thái `DELETE_FAILED`.
- Kiểm tra S3 object versions, CloudWatch Logs, SQS, SNS, WAF và Amplify.
- Theo dõi Billing/Cost Explorer trong những ngày tiếp theo vì dữ liệu chi phí có độ trễ.

> Production dùng `RETAIN` cho một số bucket, bảng và User Pool. `cdk destroy` không xóa các tài nguyên được giữ lại. Không khẳng định chi phí bằng 0 cho đến khi Billing xác nhận và không còn tài nguyên ngoài stack.
