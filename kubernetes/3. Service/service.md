# Service

Mỗi POD khi tạo ra nó có một `IP` để liên lạc, tuy nhiên vấn đề là mỗi khi POD được thay thế thì sẽ đc cấp một IP khác, nên các dịch vụ truy cập không biết IP mới nếu ta cấu hình nó truy cập đến POD nào đó cố định. Để giải quết vấn đề này sẽ cần đến `Service`.

`Service` (micro-service) là một đối tượng trừu tượng nó `xác định ra một nhóm các POD và chính sách để truy cập đến POD đó`. Nhóm các POD mà Service xác định thường dùng kỹ thuật `selector` (chọn các POD thuộc về Service `theo label của POD`).

Cũng có thể hiểu Service là một dịch vụ mạng, tạo cơ chế cân bằng tải (`load balancing`) truy cập đến các điểm cuối (thường là các Pod) mà Service đó phục vụ.

Trong `service` sẽ có `Endpoints` trỏ tới các IP của pods mà service này select (`selector`). Như vậy khi truy cập service thì nó sẽ điều phối vào các pod có `endpoints` được define. Nếu `endpoints` không có gì, có nghĩa là truy cập thì không có phản hồi nào.

## Các kiểu Service

- `ClusterIP`: kiểu mặc định, tạo ra IP để truy cập trong Cluster
- `NodePort`: có thể truy cập từ ngoài internet bằng IP của các Node, với một cổng nó ngẫu nhiên sinh ra trong khoảng `30000-32767`. Nếu muốn ấn định một cổng của Service mà không để ngẫu nhiên thì dùng tham số `nodePort` trong define service.
- `LoadBalancer`
- `ExternalName`

## Service command

- Lấy các service

> kubectl get svc -o wide

- Xem thông tin của service svc1

> kubectl describe svc/SERVICE_NAME

- Get endpoints cho services

> kubectl get endpoints