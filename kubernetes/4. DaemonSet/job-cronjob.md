# Job - CronJob

`Job` (jobs) có chức năng tạo các POD đảm bảo nó chạy và kết thúc thành công. Khi các POD do Job tạo ra chạy và kết thúc thành công thì Job đó hoàn thành.

Khi bạn xóa Job thì các Pod nó tạo cũng xóa theo.

Một Job có thể tạo các Pod chạy tuần tự hoặc song song. 

Sử dụng Job khi muốn thi hành một vài chức năng hoàn thành xong thì dừng lại (ví dụ backup, kiểm tra...)

`CronJob` (cj) - chạy các Job theo một lịch định sẵn. Việc lên lịch cho CronJob khai báo giống Cron của Linux