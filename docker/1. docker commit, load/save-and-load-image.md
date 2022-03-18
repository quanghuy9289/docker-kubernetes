# Lưu Image ra file, Nạp image từ file

Nếu muốn copy image ra máy khác có 2 cách:

- Đưa lên repository
- Lưu image ra file và load lại image từ file đó

## Lưu image ra file

> docker save --output myimage.tar myimage

- `myimage.tar`: image file được save, có thể chỉ full path cho file output (lưu ý chỉ dùng .tar extention)
- `myimage`: image muốn lưu ra file

## Load image file vào docker