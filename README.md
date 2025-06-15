# tunl

**tunl** là một dịch vụ tunnel VMess (V2Ray) chạy trên nền tảng Cloudflare Workers. Dự án cho phép triển khai proxy VMess mà không cần máy chủ riêng, rất thích hợp cho mục đích thử nghiệm hoặc khi bạn muốn tận dụng hạ tầng của Cloudflare.

## Tính năng

- Xử lý giao thức VMess và truyền dữ liệu qua WebSocket.
- Hỗ trợ chuyển tiếp TCP và DNS (qua DoH) đến máy chủ upstream.
- Trang `/link` cung cấp URL cấu hình dạng `vmess://` để dễ dàng nhập vào ứng dụng V2Ray/Xray.

## Triển khai

### Cách nhanh nhất

Nhấn nút bên dưới để tự động fork và triển khai lên tài khoản Cloudflare của bạn:

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/amiremohamadi/tunl)

Sau khi hoàn tất, truy cập `https://{TEN-SUBDOMAIN}.workers.dev/link` để lấy liên kết cấu hình.

### Triển khai thủ công

1. [Tạo API token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/) trên trang quản lý của Cloudflare.
2. Tạo file `.env` dựa trên `.env.example` và điền giá trị token:

| Biến môi trường       | Mô tả                                              |
|-----------------------|----------------------------------------------------|
| `CLOUDFLARE_API_TOKEN` | API Token dùng cho `wrangler` deploy               |

3. Triển khai bằng lệnh:

```sh
$ make deploy
```

4. Cài đặt Xray/V2Ray trên máy của bạn. Sửa file [vmess.json](./config/vmess.json) cho phù hợp (nhất là phần `address` và `id`). Khởi chạy với:

```sh
$ xray -c ./config/vmess.json
```

5. Trong ứng dụng khách (V2RayN, v2rayNG, v.v.), nhập liên kết lấy được từ bước trên vào để kết nối.

## Phát triển địa phương

Nếu muốn chạy thử worker ở môi trường local, cài đặt `wrangler` rồi dùng:

```sh
$ make dev
```

Wrangler sẽ biên dịch mã nguồn Rust sang WebAssembly và cung cấp địa chỉ truy cập thử.

## Thông tin thêm

Giá trị `UUID` mặc định được đặt trong `wrangler.toml`. Bạn có thể thay đổi biến này (trên Cloudflare Workers hoặc trong tệp `wrangler.toml`) để sinh ra liên kết cấu hình khác.

Dự án này chỉ mang tính chất minh họa. Hãy đảm bảo bạn tuân thủ luật pháp tại nơi sinh sống khi sử dụng.

