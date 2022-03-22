# Docker compose

Khi chạy một application gồm nhiều thành phần kết nối với nhau (vd PHP, MySQL, Apache HTTPD). Những thành phần này khi phân phối application chúng đều được gọi là các dịch vụ service. Như vậy các `service` thực chất là các `container` chạy đáp ứng chức năng thành phần tạo nên application.

## docker-compose.yml

File `docker-compose.yml` gần giống ý nghĩa với file `Dockerfile`, là một file text, viết với định dạng YAML (Ain’t Markup Language, đọc nhanh định dạng Định dạng YML) là cấu hình để từ đó lệnh `docker compose` sinh ra và quản lý các service (container), các network, các volume ... cho một ứng dụng hoàn chỉnh.

- Tạo và chạy các thành phần định nghĩa trong docker-compose.yml

> docker-compose up

- Dừng và xóa: image, container, mạng, volume tạo ra bởi docker-compose up

> docker-compose down

- Theo dõi Logs từ các dịch vụ

> docker-compose logs [SERVICES]

- List service đang chạy bằng docker-compose

> docker-compose ps

- Stop, start, restart các service đang chạy với docker-compose

> docker-compose stop
>
> docker-compose start
>
> docker-compose restart

- Exec vào 1 container service chạy bởi docker-compose

> docker-compose exec SERVICE_NAME bash  # SERVICE_NAME được define trong docker-compose.yaml

Note:

Nếu có bất kì container nào cần build từ `Dockerfile` (chưa được build trước đó) thì phải add thêm `--build` option vào thành `docker-compose up --build`
