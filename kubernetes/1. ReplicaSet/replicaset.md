# ReplicaSet

`ReplicaSet` là một điều khiển `Controller` - nó đảm bảo ổn định các nhân bản (số lượng và tình trạng của POD, replica) khi đang chạy.

Mặc dù có thể sử dụng ReplicaSet một cách độc lập, `tuy nhiên trong triển khai hiện nay hay dùng Deployment`, với Deployment nó sở hữu một ReplicaSet riêng.

## Cấu trúc ReplicaSet (yaml)

Define trong `spec`

- `selector`: chọn các `pod` cần quản lý theo label (`matchLabels`)
- `template`: define `pod`: name pod, containers trong pod, volume...
- `replicas`: define số lượng `pod` cần chạy

## ReplicaSet command

- Lấy các replicaset

> kubectl get rs

- Thông tin của 1 rs

> kubectl describe rs/RS_NAME

- Liệt kê các POD có nhãn được quản lý bởi rs (vd `matchLabels` là `app: rsapp`)

> kubectl get po -l "app=rsapp"

## Horizontal Pod Autoscaler (HPA) với ReplicaSet

`Horizontal Pod Autoscaler` là chế độ `tự động scale` (nhân bản POD) dựa vào mức độ hoạt động của `CPU` đối với POD, nếu một POD quá tải - nó có thể nhân bản thêm POD khác và ngược lại - số nhân bản dao động trong khoảng `min`, `max` được cấu hình

Với việc configure một HPA cho 1 RS thì số lượng `pod` được define bởi `replicas` trong RS sẽ được override bởi HPA, với số lượng `min, max` được config (vd nếu `max < replicas`  thì pod đang chạy sẽ bị `terminated` để đạt số lượng pod theo max, tương tự `min > replicas` thì pod sẽ được khởi tạo thêm để đạt số lượng min)

## HPA command

- Tạo ra một HPA để tự động scale (tăng giảm POD) theo mức độ đang làm việc CPU

> kubectl autoscale rs RS_NAME --max=2 --min=1

Có thể dùng file cấu hình (yaml) để tạo HPA

- liệt kê các HPA

> kubectl get hpa
