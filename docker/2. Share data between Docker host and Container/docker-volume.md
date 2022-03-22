# Docker Volume

Ngoài cách share data bằng cách share 1 thư mục trên máy Host, chúng ta cũng có thể tạo một ổ đĩa (`volume`) để share giữa các Containers.

`Volume` này sẽ được quản lý bởi Docker và sẽ được mount vào các Container nào cần sử dụng

Khi Container bị xóa đi thì `volume` vẫn tồn tại, cho tới khi nào chúng ta xóa `volume` đó khỏi Docker

Volume sẽ không làm tăng size của container, và volume content sẽ tồn tại bên ngoài lifecycle của một container
## Các ưu điểm của việc sử dụng Volume

So với cơ chế share data thông qua `bind mount`, việc sử dụng `volume` để lưu trữ và chia sẻ data có nhiều ưu điểm hơn:

- Dễ dàng backup, migrate
- Sử dụng Docker CLI/API để manage
- Volume có thể dùng trên cả Linux và Window containers
- An toàn khi chia sẻ
- Có thể lưu trữ volume trên máy Host hoặc Cloud thông qua Volume Driver
- Có thể khởi tạo dữ liệu trước cho volume
- High performance

## Manage volume

- List out volume list

> docker volume ls

- Tạo một volume

> docker volume create VOLUME-NAME

- Xem thông tin một volume

> docker volume inspect VOLUME-NAME

- Xóa volume

> docker volume rm VOLUME-NAME

- Xóa tất cả volume ko sử dụng bởi container nào

> docker volume prune

- Mount volume vào container

> docker run -it --mount source=VOLUME-NAME,target=CONTAINER-FOLDER image-name/id

- Tạo một volume và bind vào 1 folder trên máy HOST

> docker volume create --opt device=HOST-FOLDER --opt type=none --opt o=bind VOLUME-NAME

Lưu ý là `HOST-FOLDER` phải được sharing trong Docker (Setting -> Resources -> File Sharing)

Để gán volume này vào container thì phải dùng `-v` (không dùng `--mount` như trên)

> docker run -it -v VOLUME-NAME:CONTAINER-FOLDER image

