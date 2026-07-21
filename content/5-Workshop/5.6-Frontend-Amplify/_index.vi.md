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

1. Mở Amplify Console, chọn **Create new app** và **GitHub**.
2. Cài đặt/ủy quyền AWS Amplify GitHub App cho đúng repository. Không cần cung cấp ảnh hoặc giá trị Personal Access Token trong luồng Console.
3. Chọn repository `AWS-Project` và branch `staging`.
4. Đánh dấu ứng dụng là monorepo nếu Console hiển thị tùy chọn này.

![Kết nối Amplify với GitHub App](/images/5-Workshop/5.6-Frontend-Amplify/amplify_app_setup.png)

Ảnh chỉ minh họa màn hình ủy quyền GitHub App; repository và branch cần được chọn theo môi trường đang deploy.

## 2. Biến môi trường

Cấu hình bốn biến:

| Key | Value |
|---|---|
| `VITE_API_URL` | Invoke URL của API thuộc môi trường `staging` (hiện kết thúc bằng stage `/dev`) |
| `VITE_USER_POOL_ID` | Cognito User Pool ID |
| `VITE_CLIENT_ID` | Cognito app client ID |
| `VITE_AWS_REGION` | `ap-southeast-1` |

![Các key biến môi trường của frontend](/images/5-Workshop/5.6-Frontend-Amplify/amplify_env_setup.png)

Không thêm dấu `/` ngoài ý muốn vào API base URL và không lưu secret trong biến `VITE_*`, vì các giá trị này được đưa vào JavaScript gửi đến trình duyệt.

## 3. Build specification

CDK đặt build specification trực tiếp trên Amplify app. Cấu hình Console tương đương:

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

## 4. SPA rewrite và deploy

Thêm rewrite `200` về `/index.html` cho các đường dẫn phía client, nhưng không rewrite file tĩnh như CSS, JS, PNG, SVG, font, map hoặc JSON. Sau đó chọn **Save and deploy** và kiểm tra branch URL của `staging`.

> CloudFront distribution phía sau Amplify là thành phần do dịch vụ quản lý và không phải CloudFront distribution dành cho processed S3 bucket. Storage stack của dự án vẫn đang tắt CloudFront riêng.
