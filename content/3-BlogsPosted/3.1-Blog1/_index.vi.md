---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## Theo Dõi Và Quản Lý Chi Phí Claude Code Trên Amazon Bedrock: Giải Pháp Toàn Diện Cho Developer
Sử dụng AI trong lập trình đang là xu hướng không thể đảo ngược, và Claude Code kết hợp với Amazon Bedrock là một trong những công cụ mạnh mẽ nhất hiện nay. Tuy nhiên, khi triển khai công cụ này cho cả một đội ngũ, các nhà quản lý và kỹ sư hệ thống thường đối mặt với một câu hỏi hóc búa: Làm sao để theo dõi mức độ sử dụng, kiểm soát chi phí và đo lường hiệu suất sinh code thực tế?

May mắn thay, AWS đã cung cấp một giải pháp giám sát (Monitoring) tích hợp sẵn thông qua OpenTelemetry (OTEL) và CloudWatch. Hãy cùng khám phá kiến trúc đằng sau hệ thống này để xem bạn có thể tận dụng nó như thế nào.

## 1. Hai Lựa Chọn Kiến Trúc Giám Sát: Sidecar vs. Central

Tùy thuộc vào quy mô đội dự án và ngân sách, bạn có thể chọn triển khai hệ thống thu thập dữ liệu (Collector) theo một trong hai mô hình sau:

### Mô hình Sidecar (Tiết kiệm & Gọn nhẹ)

Nếu bạn muốn một giải pháp theo dõi không tốn phí duy trì server (khoảng $0/tháng cho hạ tầng), Sidecar là chân ái.

- Cách hoạt động: Một ứng dụng rất nhẹ bằng Go (tên là otel-helper, chỉ khoảng 15-20MB) sẽ chạy ngầm ngay trên máy tính của từng lập trình viên.
- Ưu điểm: Mỗi client sẽ gửi số liệu trực tiếp lên CloudWatch OTLP endpoint thông qua xác thực SigV4. Hệ thống hoàn toàn không cần thiết lập VPC, Load Balancer hay ECS Fargate.

### Mô hình Central (Tập trung & Dành cho Enterprise)

Nếu công ty bạn cần lưu trữ dữ liệu lịch sử lâu dài để truy vấn SQL phân tích, mô hình Central là bắt buộc.

- Cách hoạt động: Dữ liệu từ các máy client sẽ gửi về một máy chủ trung tâm (chạy ECS Fargate) đặt sau một Application Load Balancer (ALB).
- Chi phí: Nhỉnh hơn một chút, rơi vào khoảng $30–$50/tháng cho các tài nguyên AWS.
- Sức mạnh: Cho phép kích hoạt luồng xử lý phân tích dữ liệu chuyên sâu với Athena (sẽ đề cập ở phần sau).

![alt text](../../../images/3-BlogsPosted/3.1-Blog1/image1.png)

## 2. Hệ Thống Đang Lắng Nghe Những Gì?

Không chỉ là các con số khô khan, hệ thống này gửi về các dữ liệu rất thực tế phản ánh hành vi và hiệu suất của developer:

- Mức tiêu thụ & Chi phí: Theo dõi sát sao số lượng Token (input/output/cache) và ước tính chi phí AWS theo thời gian thực (metrics token.usage, cost.usage).
- Hiệu suất lập trình: Ghi nhận số lượng dòng code được thêm/xóa (lines_of_code.count), số lượt commit và Pull Request (pull_request.count).
- Hành vi sử dụng AI: Tool AI đang đưa ra các quyết định chỉnh sửa code nào? Thời gian developer thực sự "active" làm việc với AI là bao lâu?

Tất cả những thông số này được gắn thẻ (tag) chi tiết theo từng user (user.email), loại model AI đang dùng và thậm chí là phòng ban (department/team) nếu bạn sử dụng xác thực OIDC.

![alt text](../../../images/3-BlogsPosted/3.1-Blog1/image2.png)

## 3. Dashboard Trực Quan Với PromQL

Mọi dữ liệu thu thập được sẽ đổ về CloudWatch Dashboards. Nhờ sức mạnh của ngôn ngữ truy vấn PromQL, bạn sẽ có ngay một bảng điều khiển cực kỳ xịn sò với các biểu đồ:

- Tổng quan số lượng người dùng đang active và tỷ lệ Cache hit rate (giúp tiết kiệm tiền API).
- Biểu đồ chi phí và token phân bổ theo từng user hoặc từng team.
- Phân tích hiệu suất sinh code theo từng ngôn ngữ lập trình.

![alt text](../../../images/3-BlogsPosted/3.1-Blog1/image3.png)

## 4. Quản Lý Ngân Sách (Quota) - Chống "Đốt Tiền" Vô Ý

Giao cho developer một công cụ AI mạnh mẽ mà không giới hạn ngân sách là một rủi ro lớn về chi phí cloud. Giải pháp giám sát này tích hợp sẵn một cơ chế kiểm soát ngân sách rất thông minh:

- Cứ mỗi 15 phút, một hàm AWS Lambda sẽ chạy ngầm, truy vấn số liệu token từ CloudWatch và cập nhật vào một bảng DynamoDB.
- Khi một developer sử dụng Claude Code, hệ thống sẽ đối chiếu với giới hạn quota trên DynamoDB để ra quyết định "Cho phép" (Allow) hay "Chặn" (Block) cực kỳ nhanh chóng.

## 5. Mở Khóa Sức Mạnh Phân Tích Lịch Sử Với AWS Athena (Tùy chọn)

Dashboards CloudWatch bằng PromQL rất tốt cho real-time, nhưng nó có giới hạn chỉ xem lại được dữ liệu trong 7 ngày.

Nếu bạn là quản lý cấp cao muốn làm báo cáo theo quý, bạn có thể bật tính năng analytics_enabled=true (chỉ có trên mô hình Central). Lúc này, hệ thống sẽ gửi thêm log dạng EMF (Embedded Metric Format). Dữ liệu này được Kinesis Data Firehose gom lại, chuyển thành định dạng Parquet siêu tối ưu và lưu vào S3.

Từ đây, bạn có thể dùng AWS Athena để chạy các câu lệnh SQL phân tích lịch sử làm việc của hàng trăm developer trong nhiều tháng liền một cách dễ dàng.

## Tổng Kết

Dù bạn là một team nhỏ muốn dùng mô hình Sidecar miễn phí để xem nhanh số liệu, hay một doanh nghiệp cần mô hình Central với Kinesis & Athena để quản lý hàng nghìn developer, kiến trúc giám sát này của AWS đều có thể đáp ứng. Bằng cách thiết lập hệ thống telemetry này, bạn không chỉ kiểm soát được chi phí hạ tầng mà còn đo lường được xem AI thực sự đang mang lại bao nhiêu giá trị cho dự án của mình.

### Link gốc bài viết mọi người có thể tham khảo thêm:

- [AWS Solutions Library: Monitoring Claude Code with Amazon Bedrock](https://github.com/aws-solutions-library-samples/guidance-for-claude-code-with-amazon-bedrock/blob/main/assets/docs/MONITORING.md)
- [AWS Bedrock Generative AI Application Architecture | AWS Builder Center](https://builder.aws.com/content/2f2d59922DQNz3iH1pCTeudpmhv/aws-bedrock-generative-ai-application-architecture)
- [Managing AWS Distro for OpenTelemetry Collector | AWS Open Source Blog](https://aws.amazon.com/vi/blogs/opensource/managing-aws-distro-for-opentelemetry-collector-with-aws-systems-manager-distributor/)
- [Getting started with CloudWatch automatic dashboards - Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)