# Hướng dẫn cài đặt Rally
***

<a name="I."> </a> 
# I. Cài đặt cơ bản
***
<a name="1"> </a> 
## 1. Chuẩn bị môi trường
<a name="1.1"> </a> 
### 1.1 Mô hình mạng
- Mô hình đầy đủ

![Rally Topo](images/Rally.jpg)

<a name="1.2"> </a> 
### 1.2 Các tham số phần cứng đối với các node

![Rally hardware](images/Cauhinh_phancung.jpg)

## 2. Cài đặt Rally
Rally hỗ trợ 3 cách cài đặt:
 - Cài đặt từ source code
 - Cài đặt với Devstack
 - Cài đặt Rally với Docker

## 2.1. Cài đặt Rally từ source code

 - Cài đặt các công cụ:
 ```
 apt-get install  git python-pip -y
 ```

 - Cài đặt Rally thông qua script tự động:
 ```
 curl https://raw.githubusercontent.com/openstack/rally/master/install_rally.sh | bash
 ```

 Hoặc:

 ```
 git clone https://github.com/openstack/rally.git
 cd rally.git
 ./install_rally.sh
 #Nếu muốn cài rally trong thư mục chỉ định
 ./install_rally.sh --target /foo/bar
 ```

*Lưu ý: Nếu cài bằng user thường, Rally sẽ được cài trong một virtual env là ~/rally*

 - Cài đặt Rally DB:
 ```
 rally-manage db recreate
 ```

 - Tạo file `existing-keystone-v3.json` thông tin về hệ thống OpenStack sẽ kết nối tới, Rally hỗ trợ xác thực với Keystone v2 và v3, sau đây là định dạng file cấu hình sử dụng Keystone v3:
 ```
 {
    "type": "ExistingCloud",
    "auth_url": "https://10.10.10.120:5000/v3/",
    "region_name": "RegionOne",
    "endpoint_type": "public",
    "admin": {
        "username": "admin",
        "password": "Welcome123",
        "user_domain_name": "admin",
        "project_name": "admin",
        "project_domain_name": "admin"
    },
    # Nếu không sử dụng SSL cho Keystone API, set là false 
    "https_insecure": true,
    "https_cacert": ""
 }
 ```

 - Tạo 1 `deployment` cho hệ thống OpenStack được benchmark
 ```
 rally deployment create --file=existing-keystone-v3.json --name=OPS1
 ```

 Kết quả:
 ```sh
 +--------------------------------------+----------------------------+------+------------------+--------+
| uuid                                 | created_at                 | name | status           | active |
+--------------------------------------+----------------------------+------+------------------+--------+
| 30c79808-98b0-4db2-9a23-81a86e5bad01 | 2017-03-26 10:28:59.340035 | OPS1  | deploy->finished | *      |
+--------------------------------------+----------------------------+------+------------------+--------+
```
 - Kiểm tra việc kết nối:
 ```
 rally deployment check
 ```

 Kết quả:
 ```sh
 +-------------+----------+-----------+
| services    | type     | status    |
+-------------+----------+-----------+
| __unknown__ | alarming | Available |
| __unknown__ | volumev2 | Available |
| ceilometer  | metering | Available |
| cinder      | volume   | Available |
| glance      | image    | Available |
| keystone    | identity | Available |
| neutron     | network  | Available |
| nova        | compute  | Available |
+-------------+----------+-----------+
```

 - Có thể chuyển giữa các `deployment` bằng lệnh:
 ```
 rally deployment use OPS2
 ```
 `rally deployment -h` để xem thêm các tùy chọn
