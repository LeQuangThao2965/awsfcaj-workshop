---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## Tổng hợp nhanh AWS Weekly: Claude Opus 4.8, Aurora MySQL + Kiro, AWS Transform và nhiều cập nhật đáng chú ý

Tuần này AWS mang đến khá nhiều cập nhật mới, từ AI, cơ sở dữ liệu đến khả năng đánh giá hạ tầng trước khi di chuyển lên cloud. Dưới đây là những điểm mình thấy đáng chú ý.

## Claude Opus 4.8 đã có trên AWS

Mô hình mạnh nhất hiện tại của Anthropic là **Claude Opus 4.8** đã có thể sử dụng thông qua **Amazon Bedrock** và nền tảng Claude trên AWS.

Điểm nổi bật của phiên bản này là khả năng xử lý các tác vụ dài, lập trình tự động và suy luận tốt hơn. Với developer, Claude có thể đọc toàn bộ codebase, lập kế hoạch trước khi chỉnh sửa và giữ ngữ cảnh xuyên suốt một phiên làm việc dài thay vì chỉ xử lý từng đoạn code riêng lẻ.

Nếu sử dụng qua Amazon Bedrock thì vẫn được hưởng các dịch vụ quản lý của AWS như Guardrails, Knowledge Bases và khả năng quản lý dữ liệu tích hợp.

## AWS Resilience Hub thế hệ mới

AWS cũng nâng cấp **Resilience Hub** để giúp SRE và DevOps quản lý khả năng phục hồi của hệ thống tốt hơn.

Điểm mình thấy hay là service này không chỉ đánh giá mức độ sẵn sàng của ứng dụng mà còn hỗ trợ xây dựng các chính sách về SLO, Disaster Recovery, phát hiện dependency và đánh giá theo AWS Well-Architected Framework bằng AI.

Nếu đang vận hành nhiều workload thì đây sẽ là công cụ khá hữu ích để theo dõi khả năng chịu lỗi trên toàn tổ chức.

## AWS Transform có thêm khả năng đánh giá trước khi migrate

Ngoài tính năng Continuous Modernization được AWS giới thiệu trước đó, AWS Transform giờ còn hỗ trợ đánh giá trước khi di chuyển hệ thống lên AWS.

Service có thể nhập dữ liệu từ RVTools, CMDB hoặc các công cụ discovery khác để ước tính TCO, phân tích khả năng hiện đại hóa ứng dụng và đánh giá mức độ sẵn sàng chỉ trong vài phút cho mỗi repository.

Theo mình, điều này giúp doanh nghiệp có cái nhìn rõ hơn về chi phí và mức độ phức tạp trước khi bắt đầu một dự án migration.

## Aurora MySQL kết hợp với Kiro Powers

Một cập nhật khá thú vị là **Aurora MySQL** đã tích hợp với **Kiro Powers**.

Developer giờ có thể dùng ngôn ngữ tự nhiên để thực hiện nhiều tác vụ như:

* Query dữ liệu.
* Quản lý schema.
* Scale Aurora Serverless.
* Di chuyển từ RDS sang Aurora.
* Thiết lập replication.

Thay vì thao tác thủ công hoặc viết nhiều câu lệnh SQL, AI sẽ tạo sẵn các API call, SQL và cấu hình để developer xem lại trước khi thực thi.

## OpenSearch Serverless cho Agentic AI

AWS cũng giới thiệu thế hệ mới của **OpenSearch Serverless**, được tối ưu cho các ứng dụng AI Agent.

Service hỗ trợ tìm kiếm vector, mở rộng từ 0 lên hàng nghìn request mỗi giây, đồng thời tích hợp trực tiếp với các công cụ như Cursor, Claude Code, Kiro và Vercel thông qua Agent Skills.

## Kết luận

Có thể thấy hướng đi của AWS thời gian gần đây khá rõ ràng: đưa AI vào toàn bộ vòng đời phát triển phần mềm, từ viết code, đánh giá hạ tầng, quản lý cơ sở dữ liệu cho đến vận hành và hiện đại hóa ứng dụng.

Theo mình, thay vì chỉ tập trung vào việc tạo sinh mã nguồn, AWS đang xây dựng một hệ sinh thái nơi AI trở thành "đồng đội" của developer ở mọi giai đoạn phát triển phần mềm.

Nguồn: https://aws.amazon.com/blogs/aws/