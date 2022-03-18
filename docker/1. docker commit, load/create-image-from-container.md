# Tạo một container hệ điều hành centos, cài đặt thêm vào nó gói SSH Client, lưu container này lại thành image, tạo một container từ image mới đó

- Tải hệ điều hành centos về nếu chưa có

> docker pull centos

- Tạo và chạy một container đặt tên là `mycentos` từ `centos` image

> docker run -it --name mycentos centos

- Cài SSH Client vào container `mycentos`

> yum install openssh-clients

Nếu có lỗi `Failed to download metadata for repo 'appstream'` => fixed <https://techglimpse.com/failed-metadata-repo-appstream-centos-8/>

- Lưu container lại thành image (nếu container đang chạy thì cho dừng lại)

> docker stop mycentos
>
> docker commit mycentos centos-ssh:v1
>
> docker container rm mycentos (delete old container)
>
> docker run -it --name new-container centos-ssh:v1 (create container from new image)

