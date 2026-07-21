---
title: "Console - Xác thực Cognito"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Amazon Cognito trên AWS Console (tùy chọn)

> Bỏ qua bước tạo User Pool nếu Auth stack đã được deploy. Console có thể thay đổi bố cục; cần giữ đúng giá trị cấu hình thay vì phụ thuộc tên nút.

## 1. Tạo User Pool và SPA app client

1. Mở Amazon Cognito Console và chọn tạo ứng dụng mới.
2. Chọn **Single-page application (SPA)** và đặt tên có hậu tố `staging`.
3. Chỉ bật email làm sign-in identifier; bật self-registration.
4. Yêu cầu hai standard attributes: `email` và `name`.
5. Bật email verification và account recovery qua email.
6. Dùng password policy: tối thiểu 8 ký tự, có chữ hoa, chữ thường, số và ký tự đặc biệt.
7. Chọn MFA **Optional**, chỉ bật authenticator app/TOTP; không bật SMS.
8. App client là public client, không tạo client secret; bật SRP authentication.

![Cấu hình SPA, email và thuộc tính đăng ký](/images/5-Workshop/5.4-Cognito-Auth/cognito_userpool_setup.png)

## 2. Custom attribute

CDK hiện tạo `custom:role` kiểu String, mutable, độ dài 1–20. Có thể tạo thuộc tính này để mô phỏng đúng stack, nhưng ứng dụng không dùng nó làm nguồn phân quyền chính.

> **Khác biệt cần lưu ý:** Frontend và backend kiểm tra claim `cognito:groups`, không kiểm tra `custom:role`. Thuộc tính này có thể được xem là tùy chọn đối với phần Console.

## 3. User groups

Tạo hai group:

| Group | Description | Precedence |
|---|---|---|
| `admin` | Administrators with moderation access | 0 |
| `user` | Regular users | 10 |

![Hai Cognito group của dự án](/images/5-Workshop/5.4-Cognito-Auth/cognito_groups_setup.png)

Sau khi người dùng đăng ký và xác nhận email, thêm tài khoản thử nghiệm vào group phù hợp. Dự án hiện chưa có Post Confirmation trigger tự động thêm người đăng ký vào group `user`.

Ghi lại User Pool ID và App Client ID để cấu hình API Gateway và Amplify.
