---
title: "Console - Amplify Hosting"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# React Frontend trên AWS Amplify (tùy chọn)

> Frontend stack tạo Amplify app, branch, build specification, biến môi trường và SPA rewrite bằng CDK. Phần này minh họa cấu hình tương đương trên Console.

## 1. Kết nối repository

1. Đẩy mã nguồn đã kiểm thử lên repository và branch `staging` trước khi mở Amplify.
2. Mở [AWS Amplify Console](https://console.aws.amazon.com/amplify/) ở Region `ap-southeast-1`.
3. Chọn **Create new app** → **Host web app** hoặc **Deploy an app** tùy giao diện hiện tại.
4. Chọn source provider **GitHub**, sau đó chọn **Next/Continue**.
5. Nếu được yêu cầu, chọn **Install & Authorize AWS Amplify** và cấp GitHub App quyền truy cập đúng repository. Có thể chọn **Only select repositories** để giới hạn phạm vi.
6. Quay lại Amplify, chọn repository `AWS-Project` và branch `staging`.
7. Bật tùy chọn monorepo nếu Console hiển thị và nhập app root `frontend`. Nếu dùng build specification bên dưới với đường dẫn từ repository root, kiểm tra preview build để tránh lặp thành `frontend/frontend`.
8. Chọn **Next** để sang trang build settings.

![Kết nối Amplify với GitHub App](/images/5-Workshop/5.6-Frontend-Amplify/amplify_app_setup.png)

Ảnh chỉ minh họa màn hình ủy quyền GitHub App; repository và branch cần được chọn theo môi trường đang deploy.

## 2. Biến môi trường

1. Trong bước **App settings/Build settings**, mở phần **Environment variables**.
2. Chọn **Add variable** và cấu hình bốn biến:

| Key | Value |
|---|---|
| `VITE_API_URL` | Invoke URL của API thuộc môi trường `staging` (hiện kết thúc bằng stage `/dev`) |
| `VITE_USER_POOL_ID` | Cognito User Pool ID |
| `VITE_CLIENT_ID` | Cognito app client ID |
| `VITE_AWS_REGION` | `ap-southeast-1` |

![Các key biến môi trường của frontend](/images/5-Workshop/5.6-Frontend-Amplify/amplify_env_setup.png)

Không thêm dấu `/` ngoài ý muốn vào API base URL và không lưu secret trong biến `VITE_*`, vì các giá trị này được đưa vào JavaScript gửi đến trình duyệt.

3. Lấy API URL từ API Gateway stage `dev`, User Pool ID và Client ID từ Cognito thay vì gõ theo tên hiển thị.
4. Kiểm tra các biến áp dụng cho branch `staging`. Nếu có nhiều branch, không sao chép giá trị production sang staging.
5. Sau khi app đã tạo, có thể sửa lại tại **Hosting** → **Environment variables** hoặc **App settings** → **Environment variables**, rồi redeploy branch.

## 3. Build specification

CDK đặt build specification trực tiếp trên Amplify app. Cấu hình Console tương đương:

1. Ở **Build settings**, chọn **Edit** hoặc **Edit YML**.
2. Thay nội dung được auto-detect bằng cấu hình dưới đây nếu nó không nhận đúng monorepo.

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm ci
    build:
      commands:
        - npm run --workspace=frontend build
  artifacts:
    baseDirectory: frontend/dist
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

Nếu repository sử dụng `amplify.yml`, nội dung phải tương đương build specification trên; không cần duy trì đồng thời hai nguồn cấu hình khác nhau.

3. Chọn **Save** và xem lại các giá trị: `npm ci`, workspace `frontend`, output `frontend/dist`.
4. Không đổi `baseDirectory` thành `dist` trừ khi đã đặt app root là `frontend` và log build xác nhận lệnh đang chạy bên trong thư mục đó.
5. Chọn **Next**, nhập app name có hậu tố `staging` nếu được hỏi, rồi kiểm tra trang review.

## 4. SPA rewrite và deploy

1. Chọn **Save and deploy** để bắt đầu build đầu tiên.
2. Mở build đang chạy và theo dõi các pha **Provision**, **Build**, **Deploy** và **Verify**. Nếu build lỗi, đọc dòng lỗi đầu tiên trong log thay vì deploy lại không thay đổi.
3. Sau khi deploy thành công, mở **Rewrites and redirects** trong app settings.
4. Thêm rule có source dạng SPA regex loại trừ các file tĩnh, target `/index.html`, type `200 (Rewrite)`.
5. Không dùng redirect `301/302` cho route React; trình duyệt cần nhận nội dung `index.html` nhưng giữ nguyên URL.
6. Lưu rule, mở branch URL và thử truy cập trực tiếp một route phía client rồi refresh trang.
7. Mở DevTools → Network để kiểm tra frontend gọi đúng API URL `/dev`, request protected gửi header `Authorization`, và không có lỗi CORS.

## 5. Kiểm tra build tự động

1. Thực hiện một thay đổi nhỏ trên branch `staging` và push lên GitHub.
2. Xác nhận Amplify tự tạo build mới cho đúng branch.
3. Kiểm tra commit ID trong build khớp commit vừa push.
4. Nếu không muốn tự động deploy trong tài khoản lab, tắt **Auto build** sau khi kiểm tra; đây là lựa chọn vận hành và khác với mặc định của CDK hiện tại.

> CloudFront distribution phía sau Amplify là thành phần do dịch vụ quản lý và không phải CloudFront distribution dành cho processed S3 bucket. Storage stack của dự án vẫn đang tắt CloudFront riêng.
