# Kubernetes

Kubernetes (còn gọi là `k8s`) là một hệ thống để chạy, quản lý, điều phối các ứng dụng được `container hóa` trên một cụm máy (1 hay nhiều node) gọi là `cluster`

Với Kubernetes bạn có thể cấu hình để chạy các ứng dụng, dịch vụ sao cho phù hợp nhất khi chúng tương tác với nhau cũng như với bên ngoài

Bạn có thể điều chỉnh tăng giảm tài nguyên, bản chạy phục vụ cho dịch vụ (`scale`), bạn có thể cập nhật (`update`), `thu hồi update` khi có vấn đề ... Kubernetes là một công cụ mạnh mẽ, mềm dẻo, dễ mở rộng khi so sánh nó với công cụ tương tự là `Docker Swarm`

## Các thành phần của Kubernetes

Bao gồm 2 thành phần chính: `Kubernetes Master` quản lý, điều phối các `Kubernetes Node`

### Kubernetes Master

Là máy chính của `cluster`, tại đây điều khiển cả cụm máy

Các thành phần:

- `etcd`: Lưu trữ các cấu hình chung cho cả cụm máy. etcd là một dự án nguồn mở (<https://etcd.io/>) nó cung cấp dịch vụ lưu dữ liệu theo cặp key/value
- `API-server`: Cung cấp các `API Restful` để các client (như `kubectl`) tương tác với Kubernetes
- `Scheduler`: Thành phần này giúp lựa chọn Node nào để chạy các ứng dụng, căn cứ vào tài nguyên và các thành phần khác sao cho hệ thống ổn định.
- `Controller`: điều khiển trạng thái cluster, tương tác để thực hiện các tác vụ tạo, xóa, cập nhật ... các tài nguyên

### Kubernetes Node

Nơi triển khai các ứng dụng container hóa

Các thành phần:

- `Kubelet`: dịch vụ vụ chạy trên tất cả các máy (Node), nó đảm đương giám sát việc chạy, dừng, duy trì các ứng dụng chạy trên node của nó
- `Kube-proxy`: cung cấp mạng proxy để các ứng dụng nhận được traffic từ ngoài mạng vào cluster.

## Kubernetes command

- Check version của Kubernetes

> kubectl version --short

- Cluster info

> kubectl cluster-info

- Get các node đang có trên cluster

> kubectl get nodes

- Xem thông tin của node

> kubectl describe node/NODE_NAME