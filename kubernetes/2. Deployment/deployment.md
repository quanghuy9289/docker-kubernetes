# Deployment

`Deployment` quản lý một nhóm các Pod - các Pod được nhân bản, nó tự động thay thế các Pod bị lỗi hoặc không phản hồi bằng pod mới nó tạo ra. Như vậy, deployment đảm bảo ứng dụng của bạn có một (hay nhiều) Pod để phục vụ các yêu cầu.

`Deployment` sử dụng mẫu Pod (Pod `template` - chứa định nghĩa / thiết lập về Pod) để tạo các Pod (các nhân bản replica), khi `template` này thay đổi, các Pod mới sẽ được tạo để thay thế Pod cũ ngay lập tức.

Khi `Deployment` tạo ra, nó `sinh ra một ReplicasSet` và được quản lý bởi Deployment đó. Đến lượt `ReplicaSet` do Deploy quản lý lại thực hiện `quản lý (tạo, xóa) các Pod`

## Cập nhật Deployment

Khi một `Deployment được cập nhật`, thì Deployment dừng lại các Pod, `scale lại số lượng Pod về 0`, sau đó sử dụng `template mới` của Pod để tạo lại Pod.

Pod cũ không xóa hẳn cho đến khi Pod mới được chạy, quá trình này thay thế pod này diễn ra lần lượt - 1 pod được tắt đi sẽ có 1 pod mới được tạo ra cho tới khi tất cả các pod được thay thế. Cập nhật như vậy nó đảm bảo luôn có Pod đang chạy khi đang cập nhật.

Khi cập nhật, `ReplicaSet cũ` sẽ bị hủy, các pod cũ sẽ terminated theo và `ReplicaSet mới` của Deployment được tạo => các pod mới được tạo, tuy nhiên ReplicaSet cũ chưa bị xóa để có thể khôi phục lại về trạng thái trước (`rollback`).

## Cấu trúc Deployment (yaml)

Define trong `spec`. Tương tự RelicaSet

- `selector`: chọn các `pod` cần quản lý theo label (`matchLabels`)
- `template`: define `pod`: name pod, containers trong pod, volume...
- `replicas`: define số lượng `pod` cần chạy

## Deployment command

- Lấy tất cả deployment

> kubectl get deploy -o wide

- Thông tin chi tiết về deploy

> kubectl describe deploy/DEPLOY_NAME

- Xem quá trình cập nhật của deployment

> kubectl rollout status deploy/DEPLOY_NAME

- Cập nhật tài nguyên pod

> kubectl set resources deploy/DEPLOY_NAME -c=node --limits=cpu=200m,memory=200Mi

- Kiểm tra các lần cập nhật (revision)

> kubectl rollout history deploy/DEPLOY_NAME

- Xem thông tin 1 revision nào đó

> kubectl rollout history deploy/DEPLOY_NAME --revision=REVISION_NUMBER

- Rollback về 1 revision

> kubectl rollout undo deploy/DEPLOY_NAME --to-revision=REVISION_NUMBER

`Sau khi undo thì REVISION_NUMBER sẽ bị xóa khỏi history, và 1 revision mới được thêm vào`

- Quay lại bản cập nhật gần nhất trước đó

> kubectl rollout undo deploy/DEPLOY_NAME

- Scale Deployment

> kubectl scale deploy/DEPLOY_NAME --replicas=10

Có thể thực hiện auto scale với HPA tương tự như ReplicaSet
