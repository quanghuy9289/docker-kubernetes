# Ingress

`Ingress` là thành phần được dùng để điều hướng các yêu cầu traffic giao thức HTTP và HTTPS từ bên ngoài (internet) vào các dịch vụ bên trong Cluster.

`Ingress` chỉ để phục vụ các cổng, `yêu cầu HTTP, HTTPS` còn các loại cổng khác, `giao thức khác` để truy cập được từ bên ngoài thì dùng Service với kiểu `NodePort` và `LoadBalancer`

Để Ingress hoạt động, hệ thống cần một điều khiển ingress trước (`Ingress controller`), có nhiều loại: `Ngix Ingress Controller`, `HAProxy Ingress Controller`...


