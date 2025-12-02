---
title: Tin học văn phòng cơ bản

---

### **Tin học văn phòng cơ bản**

Link challenge: [Tin học văn phòng cơ bản - Cookie Arena](https://battle.cookiearena.org/challenges/digital-forensics/tin-hoc-van-phong-co-ban)

1. Phân tích cấu trúc tệp challenge.doc
- Đầu tiên khi tả giải nén file arenas2-forensics-tin-hoc-van-phong-co-ban.zip, ta sẽ nhận được một file có dạng .doc (challenge.doc)

![image](https://hackmd.io/_uploads/B1SlRq9-Wx.png)


- Ta sẽ kiểm tra Magic number của file này bằng xxd:
Công cụ xxd là một tiện ích dòng lệnh dùng để tạo và chuyển đổi hex dumps của các file.
        ```php
        xxd Challenge.doc | head -n 1
        ```

(![image](https://hackmd.io/_uploads/SJTmR55WWx.png))


- File bắt đầu bằng d0cf 11e0 a1b1 1ae1, xác nhận đây là File MS Word Document hợp lệ, file này không mở được trong máy khả năng do có dữ liệu ẩn.
1. Tìm thêm các manh mối bằng việc kiểm tra metadata và Macro
- Ta sẽ sử dụng bộ công cụ oletools để phân tích cấu trúc bên trong của file OLE
- Phân tích bằng oleid bằng lệnh sau:

![image](https://hackmd.io/_uploads/Byrr0c9Wbx.png)


- Kết quả hiển thị cho ta một manh mối rất quan trọng, ở hàng VBA Macros, chỉ ra rằng file này chứa keywords đáng ngờ và hướng ta dùng olevba và mraptor.
- Theo hướng đó, ta tiếp tục dùng olevba để tiếp cận gần với flag
1. Sử dụng công cụ olevba để trích xuất source code của VBA Macros bị ẩn bên trong tệp

```php
olevba Challenge.doc
```

- Sau khi chạy, quan sát đầu ra, ta dễ dàng thấy được flag bị ẩn trong đó:
![image](https://hackmd.io/_uploads/ryZhCqqbWe.png)
