# Stateful Set

`StatefulSets` là một tài nguyên Kubernetes được sử dụng để quản lý các `ứng dụng stateful`, nó quản lý việc triển khai (deployment) và mở rộng (scaling) của một nhóm các Pod, đồng thời nó cũng cung cấp sự `đảm bảo về thứ tự và tính duy nhất của các Pod` này.

`StatefulSet` cũng là một Controller nhưng không giống như Deployments, nó `không tạo ReplicaSet` mà `chính nó tạo Pod` với quy ước đặt tên duy nhất. Ví dụ: Nếu bạn tạo StatefulSet với bộ đếm tên (counter), nó sẽ tạo một pod với tên counter-0. Nếu tạo nhiều bản sao của một statefulset (nhiều pod), tên của chúng sẽ tăng lên như counter-0, counter-1, counter-2...

Mỗi bản sao của StatefulSet sẽ có `trạng thái riêng` và nếu sử dụng PersistentVolume thì `mỗi Pod sẽ tạo PVC (Persistent Volume Claim) riêng`. Vì vậy, một StatefulSet có 3 bản sao sẽ tạo ra 3 Pod, mỗi Pod có Volume riêng.

Với những đặc điểm ở trên thì StatefulSet thường sẽ được sử dụng tốt nhất trong những trường hợp:

- Các dịch vụ dữ liệu (Database, lưu trữ key-value...)
- Các hệ thống nhạy cảm với việc định danh (consensus-systems, leader-election clustering...)
- Và bất cứ kiến trúc nào cần việc slow roll-out và liên kết theo cụm.

## Database Master-Slave Replication

Đây là một hệ thống database bao gồm một `master DB`, và `nhiều slave replication DB`. Với `master được sử dụng cho việc ghi dữ liệu`, và `các replication DB sẽ dùng cho việc đọc dữ liệu`. Dữ liệu được ghi vào master DB sẽ được chuyển qua các replication DB để dữ liệu trên toàn hệ thống database của ta được đồng bộ với nhau.

`Hệ thống DB master-slave replication` này sẽ giúp ta `tăng hiệu suất xử lý` của ứng dụng lên nhiều, bằng cách tách ra việc ghi dữ liệu sẽ được ghi vào một DB được gọi là master, và khi đọc dữ liệu thì ta sẽ đọc từ các DB read replicas => tăng hiệu suất và tốc độ xử lý của ứng dụng.

