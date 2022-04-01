# Peristent Volume

`PersistentVolume` (pv) là một phần không gian lưu trữ dữ liệu tronnng cluster, các PersistentVolume giống với Volume bình thường tuy nhiên nó tồn tại độc lập với POD (pod bị xóa PV vẫn tồn tại), có nhiều loại PersistentVolume có thể triển khai như `NFS, Glusterfs...` tùy vào dịch vụ lưu trữ file mà chúng ta sử dụng.

`PersistentVolumeClaim` (pvc) là yêu cầu sử dụng không gian lưu trữ (`sử dụng PV`). Hình dung PV giống như Node, PVC giống như POD: POD chạy nó sử dụng các tài nguyên của NODE, `PVC hoạt động nó sử dụng tài nguyên của PV`.

Lưu ý là 1 `PV` chỉ có 1 `PVC`. Chúng ta sẽ `mount PVC vào các POD`, các POD sẽ đọc như là ổ đĩa

Nếu PVC đã `bound` với PC, nếu xoá đi PVC thì sau đó không thể tạo lại 1 PVC khác để gắn vào PV hiện tại được nữa (vì 1 PV chỉ có 1 PVC gắn vào). Lúc này PV sẽ có status là `Released`. Để `bound` lại 1 PVC khác vào PV, chúng ta phải xóa `claimRef` và update lại config cho PV

## PV-PVC command

- Liệt kê các PV, PVC

> kubectl get pv,pvc -o wide

- Thông tin chi tiết

> kubectl describe pv/PV_NAME

> kubectl describe pvc/PVC_NAME
