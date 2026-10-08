#### **Platform IO Demo**

### **By Phan Thành Đạt**

## **Mục đích**

Sử dụng môi trường phát triển tích hợp PlatformIO trong việc:

- Sử dụng thư viện mã mở có sẵn từ cộng đồng (OneButton).
- Sử dụng thư viện LED tự phát triển.
- Xử lý các thao tác nhấn nút bằng thư viện OneButton.
- Điều khiển LED trên board ESP32 DevKit V1.
- Tìm hiểu cách sử dụng các thao tác single click và double click với thư viện OneButton.

## **Phần cứng**

Dự án này sử dụng board phát triển:

### **ESP32 DevKit V1**

- Con chip ESP32 kiến trúc Xtensa, lõi kép.
- Tích hợp LED trên chân GPIO2, active level = HIGH.
- Tích hợp nút bấm BOOT trên chân GPIO0, active level = LOW.
- Trong chương trình, nút BOOT được sử dụng để điều khiển LED.

### **Kết nối phần cứng**

| Thành phần | Chân GPIO | Kết nối |
|---|---|---|
| LED | GPIO2 | LED tích hợp trên ESP32 DevKit V1 |
| Nút nhấn | GPIO0 | Nút BOOT tích hợp trên ESP32 DevKit V1 |

## **Chức năng**

Chương trình sử dụng thư viện OneButton để xử lý các thao tác nhấn nút và điều khiển LED.

Các chức năng của chương trình:

- Bấm nút một lần (single click) để bật/tắt LED.
- Bấm nút hai lần liên tiếp (double click) để chuyển LED sang chế độ nhấp nháy.
- LED nhấp nháy với thời gian 200ms.
- Chức năng nhấn giữ (long press) không được sử dụng.
- Sử dụng thư viện OneButton để nhận diện thao tác nút nhấn và khử rung phím bấm.

## **Thay đổi so với chương trình ban đầu**

Trong chương trình ban đầu, thao tác nhấn giữ được sử dụng để chuyển LED sang chế độ nhấp nháy:

    button.attachLongPressStart(btnHold);

Trong phiên bản này, chức năng nhấn giữ được thay thế bằng thao tác double click:

    button.attachDoubleClick(btnDoubleClick);

Chức năng single click vẫn được giữ nguyên:

    button.attachClick(btnPush);

Khi double click, LED được chuyển sang chế độ nhấp nháy với thời gian 200ms.

## **Nguyên lý hoạt động**

Khi chương trình chạy, OneButton liên tục kiểm tra trạng thái của nút nhấn thông qua:

    button.tick();

Khi phát hiện single click, hàm `btnPush()` được gọi để đảo trạng thái LED:

    void btnPush()
    {
        led.flip();
    }

Khi phát hiện double click, hàm `btnDoubleClick()` được gọi để chuyển LED sang chế độ nhấp nháy:

    void btnDoubleClick()
    {
        led.blink(200);
    }

Trong vòng lặp chính, hàm:

    led.loop();

được gọi liên tục để cập nhật trạng thái của LED khi LED đang ở chế độ nhấp nháy.

## **Cấu hình GPIO**

Các chân GPIO được sử dụng trong dự án:

    BTN_PIN = GPIO0
    LED_PIN = GPIO2

Cấu hình trong `platformio.ini`:

    '-D BTN_PIN=0U'
    '-D BTN_ACT=LOW'
    '-D LED_PIN=2U'
    '-D LED_ACT=HIGH'

Trong đó:

- `BTN_PIN=0U`: sử dụng GPIO0 cho nút nhấn.
- `BTN_ACT=LOW`: nút nhấn tác động ở mức LOW.
- `LED_PIN=2U`: sử dụng GPIO2 cho LED.
- `LED_ACT=HIGH`: LED sáng khi GPIO2 ở mức HIGH.

## **Thư viện sử dụng**

### **OneButton**

Dự án sử dụng thư viện OneButton để xử lý các thao tác của nút nhấn:

- Single click.
- Double click.
- Khử rung phím bấm.

Thư viện được khai báo trong `platformio.ini`:

    lib_deps =
        mathertel/OneButton @ ^2.6.1

### **LED**

Thư viện LED được sử dụng để điều khiển trạng thái của LED.

File thư viện:

    lib/LED/LED.h

Các hàm chính:

- `on()` - bật LED.
- `off()` - tắt LED.
- `flip()` - đảo trạng thái LED.
- `blink()` - chuyển LED sang chế độ nhấp nháy.
- `loop()` - cập nhật trạng thái LED.

## **Các thành phần chính trong dự án**

- `src/main.cpp`: Chương trình chính, xử lý nút nhấn và điều khiển LED.
- `lib/LED/LED.h`: Thư viện dùng để điều khiển LED.
- `platformio.ini`: Cấu hình board, chân GPIO và thư viện OneButton.
- `README.md`: Tài liệu mô tả dự án.

## **Kết quả**

Sau khi chạy chương trình:

- Single click → LED bật/tắt.
- Double click → LED nhấp nháy với thời gian 200ms.
- Single click khi LED đang nhấp nháy → chuyển LED sang trạng thái bật/tắt theo xử lý của thư viện LED.
- Long press → không được đăng ký để điều khiển LED.

## **Cách dùng Git/GitHub**

- Khởi tạo Git repository cho dự án bằng:

        git init

- Thêm các file vào Git:

        git add .

- Tạo commit:

        git commit -m "Modify LED control to use double click"

- Tạo repository Public trên GitHub.

- Kết nối repository local với GitHub:

        git remote add origin https://github.com/thanhdatphan263/thanhdat.git

- Đổi tên branch thành `main`:

        git branch -M main

- Push mã nguồn lên GitHub:

        git push -u origin main

## **Repository**

https://github.com/thanhdatphan263/thanhdat
