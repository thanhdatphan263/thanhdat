#### **Platform IO Demo**

### **By Phan Thành Đạt**

## **Mục đích**

Sử dụng môi trường phát triển tích hợp PlatformIO trong việc:

- Sử dụng thư viện mã mở có sẵn từ cộng đồng (OneButton).
- Sử dụng thư viện LED tự phát triển.
- Xử lý các thao tác nhấn nút bằng thư viện OneButton.
- Điều khiển hai LED bằng một nút nhấn.
- Sử dụng ESP32 DevKit V1 làm board phát triển.

## **Phần cứng**

Dự án này sử dụng board phát triển:

### **ESP32 DevKit V1**

- Con chip ESP32 kiến trúc Xtensa, lõi kép.
- Tích hợp LED trên chân GPIO02, active level = HIGH.
- Không sử dụng nút BOOT tích hợp trên board.
- Sử dụng thêm một LED ngoài trên test board.
- Sử dụng thêm một nút nhấn ngoài để điều khiển hai LED.

### **Kết nối phần cứng**

- LED1 sử dụng GPIO2, active level = HIGH.
- LED2 sử dụng GPIO4, active level = HIGH.
- Nút nhấn sử dụng GPIO5, active level = LOW.
- Nút nhấn sử dụng điện trở kéo lên nội của ESP32.

## **Chức năng**

Chương trình sử dụng một nút nhấn ngoài tại GPIO5 để điều khiển hai LED.

Các chức năng của chương trình:

- Bấm nút một lần (single click) để bật/tắt LED đang được chọn.
- Bấm nút hai lần liên tiếp (double click) để chuyển chế độ điều khiển giữa LED1 và LED2.
- Nhấn giữ nút (long press) để LED đang được chọn nhấp nháy với thời gian 200ms.
- LED không được chọn không bị thay đổi trạng thái.
- Sử dụng thư viện OneButton để nhận diện và khử rung phím bấm.

## **Thay đổi so với chương trình ban đầu**

- Trong chương trình ban đầu, chỉ có một LED được điều khiển bằng một nút nhấn.
- Trong phiên bản này, chương trình được mở rộng để điều khiển hai LED bằng cùng một nút nhấn.
- Thêm LED2 tại GPIO4.
- Thêm nút nhấn ngoài tại GPIO5.
- LED1 là LED built-in tại GPIO2.
- Sử dụng double click để chuyển LED đang được điều khiển.

Các hàm xử lý nút nhấn:

### **Single click**

    button.attachClick(btnPush);

Khi phát hiện single click, hàm `btnPush()` được gọi:

    void btnPush()
    {
        selectedLED->flip();
    }

### **Double click**

    button.attachDoubleClick(btnDoubleClick);

Khi phát hiện double click, hàm `btnDoubleClick()` được gọi để chuyển LED đang được điều khiển:

    void btnDoubleClick()
    {
        if (selectedLED == &led1)
            selectedLED = &led2;
        else
            selectedLED = &led1;
    }

### **Long press**

    button.attachLongPressStart(btnHold);

Khi phát hiện thao tác nhấn giữ, hàm `btnHold()` được gọi:

    void btnHold()
    {
        selectedLED->blink(200);
    }

## **Nguyên lý hoạt động**

Khi chương trình chạy, OneButton liên tục kiểm tra trạng thái của nút nhấn thông qua:

    button.tick();

Chương trình tạo hai đối tượng LED:

    LED led1(LED1_PIN, LED1_ACT);
    LED led2(LED2_PIN, LED2_ACT);

Một con trỏ được sử dụng để xác định LED đang được điều khiển:

    LED *selectedLED = &led1;

Ban đầu `selectedLED` trỏ tới LED1.

Khi phát hiện single click, chương trình đảo trạng thái của LED đang được chọn:

    selectedLED->flip();

Khi phát hiện double click, chương trình chuyển con trỏ giữa LED1 và LED2:

    LED1 ←→ LED2

Khi phát hiện long press, LED đang được chọn chuyển sang chế độ nhấp nháy:

    selectedLED->blink(200);

Trong vòng lặp chính, cả hai LED được cập nhật trạng thái:

    led1.loop();
    led2.loop();

## **Cấu hình GPIO**

Các chân GPIO được sử dụng trong dự án:

    BTN_PIN  = GPIO5
    LED1_PIN = GPIO2
    LED2_PIN = GPIO4

Cấu hình trong `platformio.ini`:

    '-D BTN_PIN=5U'
    '-D BTN_ACT=LOW'
    '-D LED1_PIN=2U'
    '-D LED1_ACT=HIGH'
    '-D LED2_PIN=4U'
    '-D LED2_ACT=HIGH'

## **Thư viện sử dụng**

### **OneButton**

Dự án sử dụng thư viện OneButton để xử lý các thao tác của nút nhấn:

- Single click.
- Double click.
- Long press.
- Khử rung phím bấm.

Thư viện được khai báo trong `platformio.ini`:

    lib_deps =
        mathertel/OneButton @ ^2.6.1

### **LED**

Thư viện LED được sử dụng để điều khiển trạng thái của hai LED.

File thư viện:

    lib/LED/LED.h

Các hàm chính:

- `on()` - bật LED.
- `off()` - tắt LED.
- `flip()` - đảo trạng thái LED.
- `blink()` - chuyển LED sang chế độ nhấp nháy.
- `loop()` - cập nhật trạng thái LED.

## **Các thành phần chính trong dự án**

- `src/main.cpp`: Chương trình chính, xử lý nút nhấn và điều khiển hai LED.
- `lib/LED/LED.h`: Thư viện dùng để điều khiển LED.
- `platformio.ini`: Cấu hình board, chân GPIO và thư viện OneButton.
- `README.md`: Tài liệu mô tả dự án.

## **Kết quả**

- Ban đầu LED1 được chọn để điều khiển.
- Single click → bật/tắt LED1.
- Double click → chuyển sang điều khiển LED2.
- Single click → bật/tắt LED2.
- Double click → quay lại điều khiển LED1.
- Long press → LED đang được chọn nhấp nháy với thời gian 200ms.

## **Cách dùng Git/GitHub**

- Khởi tạo Git repository cho dự án bằng:

        git init

- Thêm các file vào Git:

        git add .

- Tạo commit:

        git commit -m "Add OneButton two LED control"

- Tạo repository Public trên GitHub.

- Kết nối repository local với GitHub:

        git remote add origin https://github.com/thanhdatphan263/OneButton_TwoLED.git

- Đổi tên branch thành `main`:

        git branch -M main

- Push mã nguồn lên GitHub:

        git push -u origin main

## **Repository**

https://github.com/thanhdatphan263/OneButton_TwoLED
