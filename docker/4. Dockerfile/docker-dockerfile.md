# Docker file

`Dockerfile` là một file text, trong đó chứa các dòng chỉ thị để Docker đọc và chạy theo chỉ thị đó để cuối cùng bạn có một image mới theo nhu cầu.

## Build and run a Dockerfile

- Build image từ Dockerfile

> docker build -t IMAGE_NAME:TAG .

Dấu `.` ở cuối lệnh docker build ở trên, có nghĩa tìm file `Dockerfile` ở thư mục hiện tại.

Nếu sử dụng 1 file với tên khác làm Dockerfile thì chỉ ra tên file đó

> docker build -t IMAGE_NAME:TAG -f DOCKER_FILE_NAME .

Sau khi gõ lệnh này, Docker bắt đầu đọc `Dockerfile` và thực hiện từng bước của chị thị. Nó sẽ tạo container tạm, đưa vào đó các gói, dữ liệu, cấu hình ... theo chỉ thị, mỗi bước này tạo ra một lớp image. Cuối cùng nó tạo ra một image mới có tên và tag do bạn chỉ ra ở trên, lưu trong hệ thống Docker của bạn.

>Lưu ý: trong quá trình Docker build image mới từ Dockerfile, nó có thể tạo ra các image tạm thời gây rác hệ thống. Để xóa các image tạm này hãy dùng lệnh: `docker image prune`

- Tạo - chạy một container từ image vừa build

> docker run [-it] [--name CONTAINER_NAME] [-p 8080:80] [-h mywebserver] IMAGE_NAME:TAG

## Dockerfile command

### FROM

> FROM image:tag

Chỉ thị này chỉ ra image cơ sở để xây dựng nên image mới.

Để xây dựng từ image nào đó thì bạn cần đọc document của Image đó để biết trong đó đang chứa gì, có thể chạy các lệnh gì trong đó

Ví dụ, nếu bạn chọn xây dựng từ image `centos:lastest` thì bạn bắt đầu bằng hệ điều hành CentOS và bạn có thể cài đặt, cập nhật các gói với `yum`, ngược lại nếu bạn chọn `ubuntu:latest` thì trình quản lý gói của nó là `apt`

### COPY và ADD

Thêm thư mục, file vào Image

> ADD source target

- `source`: là thư mục ở máy chạy Dockerfile, chứa data cần add vào container
- `target`: là thư mục trong container, nơi data được add vào

### ENV

Thiết lập biến môi trường, tùy hệ thống hay ứng dụng yêu cầu biến môi trường nào thì căn cứ vào đó để thiết lập.

> ENV variable_name value

### RUN

Thi hành các lệnh, tương tự chạy lệnh shell trên OS từ terminal. Có 2 cách run:

> RUN lệnh-và-tham-số-cần-chạy
>
> RUN ["lệnh", "tham số1", "tham số 2" ...]

### VOLUME

Chỉ thi tạo một ổ đĩa chia sẻ được giữa các container.

> VOLUME /container_dir

- `container_dir`: thư mục trong cointainer, sẽ mount với 1 thư mục máy host khi tạo hoặc run container

Note:

- If any build steps change the data within the volume after it has been declared, those changes will be discarded.
- The VOLUME instruction does not support specifying a `host-dir` parameter. You must specify the mountpoint when you create or run the container

### USER

Thêm user được dùng khi chạy các lệnh ở chỉ thị `RUN, CMD, WORKDIR`

> USER private

### WORKDIR

Thiết lập thư mục làm việc hiện tại cho các chỉ thị `CMD, ENTRYPOINT, ADD` thi hành.

> WORKDIR container_dir

### EXPOSE

Để thiết lập cổng mà container lắng nghe, cho phép các container khác trên cùng mạng liên lạc qua cổng này hoặc để ánh xạ cổng host vào cổng này.

> EXPOSE port

### ENTRYPOINT, CMD

Chạy lệnh trong chỉ thị này khi container được chạy

> ENTRYPOINT commnad_script
>
> ENTRYPOINT ["command", "tham-số", ...]

`CMD` ý nghĩa tương tự như `ENTRYPOINT`, khác là lệnh `chạy bằng shell` của container

> CMD command param1 param2

> CMD ["tham-số1", "tham-số2"]   # thiết lập tham số cho ENTRYPOINT

## Giảm số lượng Layer hình thành nên Image

Khi build Image từ Dockerfile thì trong hệ thống cũng tồn tại thêm một số image không có tên, không tag. Một image mới được tạo ra từ mỗi chỉ thị trong Dockerfile. Image sau sẽ kế thừa từ image trước tạo thành một chuỗi các image layer. Do tính kế thừa như vậy nên các image cha không thể xóa được.

Có thể kiểm tra số lượng image đã được tạo ra khi build image:

> docker history IMAGE_NAME:TAG

Chúng ta cố gắng xây dựng Image có ít layer nhất có thể để giảm số lượng image sinh ra ko cần thiết. Muốn ít layer thì cần viết sao cho ít chỉ thị nhất
