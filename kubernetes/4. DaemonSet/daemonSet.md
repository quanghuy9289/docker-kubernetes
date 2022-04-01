# DaemonSet

`DaemonSet` (ds) đảm bảo chạy trên `mỗi NODE 1 POD`. Triển khai DaemonSet khi cần ở mỗi máy (Node) một POD, thường dùng cho các ứng dụng như thu thập log, tạo ổ đĩa trên mỗi Node...

## DaemonSet command

- Liệt kê các DaemonSet

> kubectl get ds -o wide

- Liệt kê các POD theo nhãn

> kubectl get pod -o wide -l "app=ds-nginx"

- Chi tiết về ds

> kubectl describe ds/DS_NAME

- Xóa DaemonSet

> kubectl delete ds/DS_NAME

