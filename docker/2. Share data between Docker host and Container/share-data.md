# Chia sẻ dữ liệu giữa Docker Host và Container

`Máy Host` là hệ thống bạn đang chạy Docker Engine. Một thư mục của máy Host có thể chia sẻ để các Container đọc, lưu dữ liệu.

## Share data between Host and Container

- Giả sử có 1 thư mục trên máy Host là `path_to_folder_in_host`
- Để share thư mục này cho container, chúng ta sẽ `mount` (ánh xạ) nó tới 1 thư mục trong container `path_to_folder_in_container`. Nghĩa là nếu chúng ta lưu trữ data trong container ở folder `path_to_folder_in_container` thì data đó cũng sẽ lưu trữ trong folder `path_to_folder_in_host` ở máy host (2 folder này lúc này là 1)

> docker run -it -v path_to_folder_in_host:path_to_folder_in_container image-name

Vd:
> docker run -it -v /home/sitesdata:/home/data ubuntu

- `/home/sitesdata/`: thư mục trên máy Host
- `/home/data`: thư mục trong container

## Share data between Containers

Nếu 1 container (có id hoặc name `container-first`) có share data với máy Host, 1 container khác muốn share data giống như vậy thì có thể dùng lệnh

> docker run -it --volumes-from container-first imageName

Giả sử `container-first` có folder `/home/data` mount với `/home/sitesdata/` của máy Host như vd trên, lúc này `container-second` cũng sẽ có folder `/home/data` mount với `/home/sitesdata/` của máy Host

> /home/data (container-first) = /home/data (container-second) = /home/sitesdata/ (host)