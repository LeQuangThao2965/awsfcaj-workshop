---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cloud-based Digital Product Marketplace with 3D Preview
## Giải pháp Marketplace cho sản phẩm số tích hợp xem trước 3D, thanh toán thời gian thực và lưu trữ trên AWS  

### 1. Tóm tắt điều hành
*Về dự án* <br>
Dự án “Cloud-based Digital Product Marketplace with 3D Preview on AWS” là một nền tảng marketplace dành cho sản phẩm số như tài liệu PDF/Word, mô hình 3D, asset thiết kế và các tài nguyên kỹ thuật số khác. 


Hệ thống cho phép người dùng đăng ký tài khoản, đăng nhập, tìm kiếm sản phẩm, xem thông tin chi tiết, thanh toán trực tuyến và truy cập sản phẩm sau khi giao dịch được xác nhận thành công. 
Điểm nổi bật của dự án là khả năng hiển thị mô hình 3D trực tiếp trên giao diện web đối với các sản phẩm dạng 3D Model. Người dùng có thể xoay, phóng to, thu nhỏ và quan sát sản phẩm trước khi mua, giúp tăng tính trực quan so với các website bán tài liệu số thông thường.


Về mặt kỹ thuật, hệ thống sử dụng React ở phía Frontend, Node.js kết hợp Express ở phía Backend và database để lưu thông tin người dùng, sản phẩm, đơn hàng, giao dịch và quyền truy cập. Hạ tầng triển khai dự kiến sử dụng Amazon EC2 để chạy ứng dụng Backend, Amazon S3 để lưu trữ file sản phẩm, AWS IAM để kiểm soát quyền truy cập tài nguyên AWS và SePay để thử nghiệm quy trình thanh toán thời gian thực.

*Mục tiêu*
<br>
Mục tiêu của dự án là áp dụng các kiến thức AWS đã học trong workshop vào một bài toán thực tế, bao gồm triển khai ứng dụng web lên cloud, quản lý file bằng object storage, thiết kế phân quyền truy cập tài nguyên, tối ưu chi phí vận hành và xây dựng hệ thống có khả năng mở rộng trong tương lai.
  

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Nhiều nền tảng chia sẻ hoặc mua bán sản phẩm số chỉ tập trung vào việc đăng tải file và xử lý giao dịch cơ bản. Đối với các sản phẩm có tính trực quan cao như mô hình 3D, asset game hoặc tài nguyên thiết kế, người mua thường khó đánh giá chất lượng nếu chỉ xem ảnh minh họa hoặc mô tả bằng chữ.

Nếu toàn bộ file sản phẩm được lưu trực tiếp trên server ứng dụng, hệ thống sẽ gặp nhiều hạn chế như khó mở rộng dung lượng, phụ thuộc vào ổ cứng máy chủ, khó quản lý file khi số lượng sản phẩm tăng và tiềm ẩn rủi ro mất dữ liệu khi server gặp sự cố.

Bên cạnh đó, quy trình xác nhận thanh toán thủ công gây bất tiện cho cả người mua và người bán. Nếu quản trị viên phải kiểm tra chuyển khoản bằng tay, thời gian xác nhận đơn hàng sẽ chậm, dễ sai sót và không phù hợp với mô hình marketplace tự động.

*Giải pháp*  
Dự án đề xuất xây dựng một marketplace cho sản phẩm số, trong đó ứng dụng web chịu trách nhiệm quản lý người dùng, sản phẩm, giao dịch và quyền truy cập sau khi mua. Các file sản phẩm được lưu trữ tách biệt trên Amazon S3 thay vì lưu trực tiếp trên server. Backend Node.js + Express đóng vai trò trung gian xử lý nghiệp vụ, xác thực người dùng, quản lý sản phẩm, kiểm tra giao dịch và giao tiếp với các dịch vụ AWS.

Đối với sản phẩm dạng 3D Model, Frontend React tích hợp chức năng 3D Viewer để hiển thị mô hình trực tiếp trên web. Đối với tài liệu PDF/Word, hệ thống sẽ tiếp tục hoàn thiện chức năng xem trước nội dung trước khi mua, chẳng hạn giới hạn số trang preview hoặc hiển thị bản xem thử.

Luồng thanh toán được tích hợp với SePay nhằm tự động nhận thông báo giao dịch theo thời gian thực. Khi người dùng chuyển khoản thành công, Backend xử lý dữ liệu webhook/API từ SePay, đối chiếu mã đơn hàng, cập nhật trạng thái giao dịch và mở quyền truy cập sản phẩm cho người mua.

**Lợi ích và giá trị mang lại:** 
<br>&emsp;- Mô phỏng một hệ thống marketplace thực tế với các luồng người mua, người bán và quản trị viên. 
<br>&emsp;- Tách biệt compute và storage bằng cách triển khai ứng dụng trên EC2 và lưu file trên S3.
<br>&emsp;- Tăng trải nghiệm người dùng nhờ khả năng xem trước mô hình 3D trên web.
<br>&emsp;- Tự động hóa xác nhận thanh toán thông qua SePay, giảm thao tác kiểm tra thủ công.
<br>&emsp;- Áp dụng IAM và nguyên tắc least privilege để kiểm soát quyền truy cập tài nguyên AWS.


### 3. Kiến trúc giải pháp  
Hệ thống được thiết kế theo mô hình web application triển khai trên AWS, bao gồm các thành phần chính: Client, Frontend, Backend, Database, Amazon S3, SePay và IAM. Người dùng truy cập giao diện React; Frontend gửi request đến Backend Node.js + Express thông qua REST API; Backend xử lý nghiệp vụ, kết nối database, giao tiếp với S3 và xử lý thanh toán thông qua SePay.
```
Luồng tạm thời do chưa vẽ
User Browser
    -> React Frontend
        -> Node.js + Express Backend on Amazon EC2
            -> Database
            -> Amazon S3 (PDF, Word, images, 3D models)
            -> SePay API / Webhook
            -> AWS IAM Role / Policy
```
{{% notice note %}}
**Note:** Sơ đồ kiến trúc chính thức sẽ được bổ sung trong báo cáo cuối sau khi hoàn tất cấu hình triển khai thử nghiệm trên AWS.
{{% /notice %}}

![IoT Weather Station Architecture](../../images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](../../images/2-Proposal/platform_architecture.jpeg)

***Dịch vụ AWS sử dụng***
- **React**: Xây dựng giao diện người dùng, trang sản phẩm, trang chi tiết, trang quản trị và khu vực xem trước 3D
- **Node.js + Express**: Xây dựng Backend API, xử lý nghiệp vụ marketplace, xác thực, quản lý sản phẩm, giao dịch và tích hợp dịch vụ ngoài.
- **Database**: Lưu dữ liệu người dùng, sản phẩm, danh mục, đơn hàng, giao dịch và quyền truy cập sản phẩm.
- **Amazon EC2**: Môi trường triển khai Backend, chạy Node.js + Express và xử lý request từ Frontend.
- **Amazon S3**: Lưu trữ file sản phẩm số như PDF, Word, ảnh, thumbnail và mô hình 3D.
- **AWS IAM**: Quản lý quyền truy cập S3 và giới hạn quyền theo nguyên tắc least privilege.
- **AWS Budget**: Theo dõi và cảnh báo chi phí trong quá trình thử nghiệm.
- **SePay**: Tích hợp thanh toán và xử lý thông báo giao dịch theo thời gian thực.
- **GitHub / GitHub Actions**: Quản lý mã nguồn và có thể mở rộng sang CI/CD trong giai đoạn sau.


***Thiết kế thành phần***  
- **Frontend React**: hiển thị giao diện người dùng, danh sách sản phẩm, chi tiết sản phẩm, quản trị user và viewer 3D.
- **Backend Node.js + Express**: xử lý REST API, validate dữ liệu, kiểm tra quyền truy cập, thao tác database, gọi SePay và S3.
- **Database**: lưu các bảng users, products, categories, orders, transactions, product_files và user_purchases.
- **Amazon S3**: lưu các object theo cấu trúc logic như products/{productId}/thumbnail.jpg, products/{productId}/document.pdf và products/{productId}/model.glb.
- **A**dmin**: quản lý người dùng, ban/unban tài khoản, kiểm tra sản phẩm và theo dõi trạng thái hệ thống.
- **Seller**: đăng sản phẩm, cập nhật sản phẩm và tải file sản phẩm lên hệ thống.
- *Buyer*: tìm kiếm, xem trước, thanh toán và truy cập sản phẩm sau khi mua.


### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Giai đoạn   |   Nội dung thực hiện
|---|---|
|Giai đoạn 1: Nghiên cứu và lựa chọn đề tài | Tìm hiểu EC2, S3, IAM, AWS Budget và quy trình deploy ứng dụng web; lựa chọn đề tài marketplace sản phẩm số tích hợp 3D Preview.|
|Giai đoạn 2: Thiết kế kiến trúc và khởi tạo dự án | Khởi tạo React + Node.js + Express; xây dựng cấu trúc frontend/backend; xác định các module Authentication, Product Management, User Management, Payment và File Storage.|
|Giai đoạn 3: Phát triển chức năng cốt lõi | Phát triển đăng ký, đăng nhập, tìm kiếm sản phẩm, hiển thị sản phẩm, quản lý user, ban/unban và hiển thị 3D model.|
|Giai đoạn 4: Tích hợp thanh toán và lưu trữ file | Tích hợp SePay cho thanh toán thời gian thực; chuẩn bị luồng upload và quản lý file sản phẩm trên Amazon S3.|
|Giai đoạn 5: Triển khai AWS và kiểm thử | Deploy Backend lên EC2, cấu hình S3 bucket, IAM Policy/Role, kiểm thử các luồng chính và hoàn thiện báo cáo.|


*Yêu cầu kỹ thuật*  
- Backend hỗ trợ REST API, xác thực bằng session/JWT, validate dữ liệu, xử lý upload file, kết nối database và giao tiếp với SePay/S3.
- Frontend hỗ trợ giao diện responsive, danh sách sản phẩm, form đăng nhập/đăng ký, trang chi tiết sản phẩm, trang quản trị và 3D Viewer.
- AWS cần có EC2 instance cho môi trường thử nghiệm, Security Group mở các port cần thiết, S3 bucket lưu file sản phẩm, IAM policy giới hạn quyền truy cập và AWS Budget để theo dõi chi phí.
- Hệ thống cần kiểm tra quyền truy cập sau khi mua để đảm bảo người dùng chỉ tải/xem đầy đủ sản phẩm đã thanh toán thành công.

*Trạng thái nguyên mẫu hiện tại*
- Đã có chức năng đăng ký, đăng nhập và tìm kiếm sản phẩm.
- Đã có giao diện hiển thị 3D View Model đối với sản phẩm dạng 3D Model.
- Đã có chức năng quản lý user cơ bản, bao gồm ban/unban user.
- nĐã tích hợp thanh toán thời gian thực thông qua SePay và xử lý thông báo chuyển tiền để xác nhận giao dịch thành công.
- Chưa hoàn thiện deploy AWS, sơ đồ kiến trúc chính thức, đăng ký seller, quản lý danh mục và chức năng preview tài liệu trước khi mua.



### 5. Lộ trình & Mốc triển khai  
Mốc thời gian | Mục tiêu chính
|---|---|
|Tháng 1 | Học nền tảng AWS, thực hành EC2/S3/IAM/AWS Budget, thử nghiệm deploy ứng dụng mẫu và xác định đề tài.|
|Tháng 2 | Thiết kế kiến trúc EC2 + S3 + IAM, khởi tạo project React + Node.js + Express, xây dựng database và phát triển MVP.|
|Tháng 3 | Tích hợp thanh toán SePay, hoàn thiện luồng buyer/seller/admin, deploy thử nghiệm lên EC2, tích hợp S3 và hoàn thiện báo cáo.|

- *Sau triển khai*: Tiếp tục nghiên cứu và phát triển sản phẩm thêm 1 năm.  

### 6. Ước tính ngân sách  

Dự án được thiết kế theo hướng tiết kiệm chi phí trong môi trường học tập và thử nghiệm. Các dịch vụ chính gồm Amazon EC2, Amazon S3, AWS IAM và AWS Budget. Chi phí thực tế sẽ được cập nhật bằng AWS Pricing Calculator sau khi chốt Region, instance type, dung lượng S3 và thời gian chạy EC2.
```
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  
Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).  
```
```
*Chi phí hạ tầng*  
- AWS Lambda: 0,00 USD/tháng (1.000 request, 512 MB lưu trữ).  
- S3 Standard: 0,15 USD/tháng (6 GB, 2.100 request, 1 GB quét).  
- Truyền dữ liệu: 0,02 USD/tháng (1 GB vào, 1 GB ra).  
- AWS Amplify: 0,35 USD/tháng (256 MB, request 500 ms).  
- Amazon API Gateway: 0,01 USD/tháng (2.000 request).  
- AWS Glue ETL Jobs: 0,02 USD/tháng (2 DPU).  
- AWS Glue Crawlers: 0,07 USD/tháng (1 crawler).  
- MQTT (IoT Core): 0,08 USD/tháng (5 thiết bị, 45.000 tin nhắn).  

*Tổng*: 0,7 USD/tháng, 8,40 USD/12 tháng  
- *Phần cứng*: 265 USD một lần (Raspberry Pi 5 và cảm biến).  
```

### 7. Đánh giá rủi ro  
Rủi ro|	Ảnh hưởng|	Xác suất|	Chiến lược giảm thiểu
|---|---|---|---|
|Chưa triển khai thành công lên AWS	|Cao	|Trung bình	|Ưu tiên deploy Backend lên EC2 trước, sau đó mới tích hợp S3 và các phần phụ.|
|Tích hợp S3 gặp lỗi quyền truy cập	|Trung bình	|Trung bình	|Thiết kế IAM policy rõ ràng và kiểm thử từng quyền PutObject, GetObject, DeleteObject.|
|Preview PDF/Word chưa hoàn thiện	|Trung bình	|Cao	|Ưu tiên preview PDF trước; với Word có thể dùng bản PDF preview hoặc thumbnail thay thế.|
|Sai lệch trạng thái giao dịch thanh toán	|Cao	|Trung bình	|Kiểm tra mã đơn hàng, số tiền, trạng thái giao dịch và xử lý chống trùng webhook.|
|Vượt chi phí AWS khi thử nghiệm	|Trung bình	|Thấp	|Bật AWS Budget, giới hạn dung lượng S3 và tắt EC2 khi không sử dụng.|


### 8. Kết quả kỳ vọng  
- Xây dựng được marketplace sản phẩm số với các chức năng cơ bản như đăng ký, đăng nhập, tìm kiếm, xem chi tiết sản phẩm, quản lý user và quản lý sản phẩm.
- Hỗ trợ hiển thị mô hình 3D trực tiếp trên web đối với sản phẩm dạng 3D Model.
- Tích hợp quy trình thanh toán thời gian thực thông qua SePay và tự động cập nhật trạng thái giao dịch.
- Triển khai thử nghiệm Backend lên Amazon EC2 và sử dụng Amazon S3 để lưu trữ file sản phẩm số.
- Áp dụng AWS IAM để kiểm soát quyền truy cập tài nguyên cloud theo nguyên tắc bảo mật cơ bản.
- Hình thành kiến trúc hệ thống rõ ràng, phù hợp để trình bày trong báo cáo workshop và có khả năng mở rộng trong tương lai.
Về giá trị học tập, dự án giúp sinh viên hiểu rõ hơn quy trình triển khai một ứng dụng web thực tế lên AWS, cách tách biệt compute và storage, cách thiết kế API backend, cách xử lý thanh toán online và cách kiểm soát quyền truy cập tài nguyên theo nguyên tắc bảo mật cơ bản.

Về khả năng mở rộng, hệ thống có thể phát triển thêm các chức năng như đăng ký seller, duyệt sản phẩm trước khi đăng bán, quản lý danh mục nâng cao, đánh giá sản phẩm, ví người bán, chia doanh thu, preview tài liệu trước khi mua và CI/CD tự động bằng GitHub Actions.
