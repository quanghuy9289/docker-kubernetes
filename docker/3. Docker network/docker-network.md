# Docker Network

Docker cho phép tạo ra 1 `network`, sau đó các container kết nối vào `network` đó

Các container cùng `network` có thể communicate với nhau thông qua IP được cấp cho container đó (hoặc dùng `Container name` và `listening port` của container trên mạng đó)

## Network driver

Docker’s networking subsystem is `pluggable`, using `drivers`

Các `network` được tạo ra theo một `driver` nào đó: `bridge, host, overlay, ipvlan, macvlan, none`

- `bridge`: default network driver, cho các ứng dụng cùng network giao tiếp với nhau và gởi gói tin ra ngoài. Docker sẽ tạo ra một switch ảo. Khi container được tạo ra, interface của container sẽ gắn vào switch ảo này và kết nối với interface của host.
- `host`: sử dụng chung network với máy host
- `overlay`: cho phép kết nối nhiều `Docker daemons` và cho phép `swarm services` giao tiếp với chúng.
- `macvlan`: cho phép cấu hình `sub-interfaces` trên một Ethernet interface vật lý. Mỗi `sub-interface` này có địa chỉ MAC riêng và địa chỉ IP riêng. Các containers có thể kết nối với một `sub-interface` nhất định để kết nối trực tiếp với mạng vật lý, sử dụng địa chỉ MAC và địa chỉ IP riêng của `sub-interface` đó. Ta có thể cấu hình nhiều `sub-interfaces` để chia sẻ cùng 1 Ethernet interface vật lý, và cấu hình 1 dải IP dành cho 1 `sub-interface` nhất định khi tạo 1 macvlan. MacVlan có 4 kiểu (Bridge, VEPA, Private, Passthrough). Bridge là kiểu hay được sử dụng nhất.
- `ipvlan`: Ipvlan khá giống so với macvlan, tuy nhiên nó có điểm khác so với macvlan là không gán địa chỉ MAC riêng cho các sub-interfaces. Các sub-interfaces chia sẻ chung địa chỉ MAC với parent interfaces (card vật lý trên đó tạo các sub-interfaces), nhưng có địa chỉ IP riêng.
- `none`: Các container thiết lập network này sẽ không được cấu hình mạng.
- `Network plugins`: Sử dụng 1 3rd party network để làm driver

`Docker daemon` (Docker Engine): là 1 server Docker, xử lý các request từ `Docker Client` (Docker API/CLI). Nó quản lý `image, container, network, volume`

`Swarm service`: là một công cụ cho phép quản lý, scale... các `Docker daemon` bằng cách quản lý theo cụm - `cluster`. Ta có thể gom một số Docker host lại với nhau thành dạng cụm (cluster) và ta có xem nó như một máy chủ Docker ảo (`virtual Docker host`) duy nhất. Một Swarm là một cluster của một hoặc nhiều Docker Engine đang chạy. `Swarm mode` cung cấp cho ta các tính năng để quản lý và điều phối cluster.

## Manage network

- List out network

> docker network ls

- Tạo 1 bridge network

> docker network create --driver bridge NETWORK-NAME

- Tạo container sử dụng network

> docker run --network NETWORK-NAME --name CONTAINER-NAME IMAGE

Lúc này container mới tạo sẽ được cung cấp một IPv4/v6 cụ thể. Các container cùng network NETWORK-NAME này có thể giao tiếp với nhau thông qua IP address này

- Connect container vào network chỉ định

> docker network connect NETWORK-NAME CONTAINER-NAME

- Kiểm tra xem một network có container nào connect vào

> docker network inspect NETWORK-NAME

- Kiểm tra xem 1 container đang connect vào network nào

> docker inspect CONTAINER-NAME/ID

- Thiết lập network port khi tạo container

> docker run -p PUBLIC_PORT:TARGET_PORT/protocol...

`PUBLIC_PORT` (cổng trên máy host): cổng public ra ngoài (80, 8080...), các kết nối không cùng network đến container phải thông qua cổng này.

`TARGET_PORT` (cổng bên trong container): cổng `PUBLIC_PORT` sẽ ánh xạ vào cổng này. Nếu container cùng network có thể kết nối với nhau thông qua `TARGET_PORT` này

- Xóa network

> docker network rm NETWORK-NAME

