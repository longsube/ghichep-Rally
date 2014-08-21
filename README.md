# Đo hiệu năng node vật lý bằng Rally

# 1. Rally là gì?
Rally là một công cụ để tự động hóa và hợp nhất các bước cần thiết cho việc đánh giá hệ thống OpenStack ở quy mô lớn: triển khai nhiều node, xác thực, benchmark.

# 2. Use cases

<img src=http://i.imgur.com/npnYGA6.png width="60%" height="60%" border="1">

Các mục tiêu mà Rally hướng tới trong các use cases:
 1/ Tự động đo và đánh giá code mới tác động tới hiệu năng của OS như thế nào.
 2/ Tìm ra những vấn đề trong việc scaling tới hiệu năng hệ thống.
 3/ Kiểm tra những cách triển khai khác nhau tác động đến hiệu năng hệ thống thế nào:
	-Tìm ra mô hình triển khai hợp lý nhất.
	-Thông sô cấu hình khác nhau cho những tải khác nhau (tổng số controller, swift node,...)
 4/ Tự động tìm kiếm phần cứng phù hợp nhất cho đám mây OpenStack.
 5/ Tự động cấu hình thông số hoạt động của đám mây:
	-Quyết định tải cuối cùng cho hệ thống cloud cơ bản.
	-Kiểm tra hiệu năng của hệ thống cloud cơ bản ở nhiều mức tải khác nhau.
# 3. Kiến trúc

Sử dụng 2 mô hình:
 1/ Rally-as-a-service: Rally được cài đặt gồm nhiều thành phần và biểu diễn dưới dạng Web UI.
 2/ Rally-as-an-app: Rally được cài đặt như 1 app, đơn giản hóa việc cài đặt và phát triển.
 
<img src=http://i.imgur.com/GEzRl3R.png width="60%" height="60%" border="1">

# 4. Các thành phần trong Rally
 1/ Service Provider: cung cấp các server ảo với kết nối ssh trên L3 network.
 2/ Deploy Engines: triển khai đám mây OpenStack trên các server được tạo bởi Service Provider.
 3/ Verification: thành phần chạy các kịch bản test, thu thập kết quả và biểu diễn dưới dạng đồ thị.
 4/ Benmark Engine: cho phép tạo các kịch bản benchmark.
 
# 5. Cài đặt Rally-as-an-app
## 5.1. Tải về source code của Rally
     git clone https://git.openstack.org/stackforge/rally
## 5.2. Cài đặt Rally
    ./rally/install_rally.sh 	 
 