#### **Platform IO: OneButton Library Demo**

### **By Phan Thành Đạt**


## **Mục đích**

Sử dụng môi trường phát triển tích hợp PlatformIO trong việc:

    - Sử dụng thư viện mã mở có sẵn từ cộng đồng (OneButton).
    - Sử dụng thư viện (LED).
    - Xử lý các thao tác nhấn nút bằng thư viện OneButton.
    - Phát triển chương trình điều khiển LED trên ESP32.
## Phần cứng

Dự án này sử dụng board phát triển:

    ESP32 Devkit v1:
        - Con chip ESP32 kiến trúc xtensa, lõi kép.
        - Tích hợp Blue LED vào chân GPIO02, active level = HIGH.
        - Tích hợp nút bấm (BOOT) vào chân GPIO00, active level = LOW.

## Chức năng

Chương trình sử dụng thư viện OneButton để xử lý thao tác nhấn nút và điều khiển LED.

Các chức năng của chương trình:

        - Bấm nút một lần (single click) để bật/tắt LED (đảo trạng thái).
        - Bấm nút hai lần liên tiếp (double click) để LED chuyển sang trạng thái nhấp nháy liên tục         (blink 200ms một lần).
        - Chức năng nhấn giữ (long press) không còn được sử dụng để điều khiển LED nhấp nháy.
        - Sử dụng thư viện OneButton để nhận diện và khử rung phím bấm.
Thay đổi so với chương trình ban đầu

        - Trong chương trình ban đầu, thao tác nhấn giữ được sử dụng để chuyển LED sang trạng thái nhấp     nháy:

                button.attachLongPressStart(btnHold);

        - Trong phiên bản này, chức năng trên được thay thế bằng thao tác double click:

                button.attachDoubleClick(btnDoubleClick);

        - Chức năng single click vẫn được giữ nguyên:

                button.attachClick(btnPush);
        - Các thành phần chính trong dự án:
    + src/main.cpp: Chương trình chính, xử lý nút nhấn và điều khiển LED.
    + lib/LED/LED.H: Thư viện dùng để điều khiển LED.
    + platformio.ini: Cấu hình board, chân GPIO và thư viện OneButton.
    + README.md: Tài liệu mô tả dự án.
## Nguyên lý hoạt động

Khi chương trình chạy, OneButton liên tục kiểm tra trạng thái của nút nhấn thông qua:

        button.tick();

Khi phát hiện single click, hàm btnPush() được gọi để đảo trạng thái LED:

        void btnPush()
        {
            led.flip();
        }

Khi phát hiện double click, hàm btnDoubleClick() được gọi để chuyển LED sang chế độ nhấp nháy:

        void btnDoubleClick()
        {
            led.blink(200);
        }
## Kết quả
        - Single click → LED bật/tắt.
        - Double click → LED nhấp nháy với thời gian 200ms.
        - Long press → không còn kích hoạt chế độ nhấp nháy.

## Cách dùng git/github


    - Khởi tạo Git repository cho dự án bằng
        git init

    - Thêm các file vào Git
        git add .

    - Tạo commit:
        git commit -m "Modify LED control to use double click"

    - Sau đó tạo repository Public trên GitHub và push mã nguồn lên repository
        git remote add origin <https://github.com/thanhdatphan263/thanhdat>
        git branch -M main
        git push -u origin main
