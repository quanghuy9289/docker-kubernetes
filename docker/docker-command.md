# Common Docker commands

## Kiểm tra phiên bản

> docker --version

---

## Docker images

### Liệt kê các image

> docker images -a

### Xóa một image (không có container nào đang dùng)

> docker images rm imageid

### Tải về một image (imagename) từ hub.docker.com

> docker pull imagename:tag

Ignore tag will get the latest image

### Lưu image ra file

> docker save --output myimage.tar myimage

### Load image vào Docker

> docker load -i myimage.tar

### Gắn tag cho image

> docker tag image-name imagename:version

### Đổi tên image

> docker tag image-id imagename:version

---

## Docker containers

### Liệt kê các containers

> docker container ls -a

### Xóa container

> docker container rm containerid

### Tạo mới một container từ image

> docker run -it imageid

- `-it`: `-t` - terminal, `-i` - stdin (nhận lệnh input)
- Nếu không có `-it` option thì run ở background

### Thoát termial vẫn giữ container đang chạy

> CTRL +P, CTRL + Q

### Vào termial container đang chạy

> docker container attach containerid

### Chạy lại container đã dừng

> docker container start -i containerid

### Chạy một lệnh trên container đang chạy

> docker exec -it containerid command

### Tạo image từ container

Khi sử dụng Container bạn có thể cấu hình, cài đặt thêm vào nó các package, đưa thêm dữ liệu ...

> docker commit mycontainer myimage:version

- mycontainer: tên hoặc id của container
- myimage, version: tên và phiên bản image. Nếu lưu cùng tên với image tạo ra container này, coi như image cũ được cập nhật mới.

---
