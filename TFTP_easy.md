### Trivial Flag Transfer Protocol

Link challenge: [Trivial Flag Transfer Protocol - Cookie Arena](https://battle.cookiearena.org/challenges/digital-forensics/trivial-flag-transfer-protocol)

- Đầu tiên, ta sẽ xem quá trình truyền file giữa client và server bằng chức năng export Objects của Wireshark theo giao thức TFTP:
- Ta sẽ thu được rất nhiều file bao gồm file hai file text và các file ảnh .bmp (khả năng là dữ liệu bị ẩn bên trong):

![image](https://hackmd.io/_uploads/ryz2qJs-Wg.png)


Hình 1. Các file thu được trong quá trình truyền file  bằng wireshark

- Ta sẽ lần lượt mở từng file instructions.txt và plan để xem nội dung bên trong có gì:
    - Đầu tiên là file instructions.txt:
        
        ```php
        GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
        
        ```
        
        Hình 2. Nội dung bên trong file instructions.txt
        
    - Nhìn thoáng qua, ta sẽ thấy nội dung này có thể đã được mã hóa bằng ROT13 (một dạng mã hóa dịch chuyển đơn giản, trong đó mỗi chữ cái được thay thế bằng chữ cái cách nó 13 vị trí trong bảng chữ cái).
    - Đưa nội dung này vào công cụ ROT13 Decoder

![image](https://hackmd.io/_uploads/rksCc1oWZx.png)

   Hình 3. Plaintext của file instructions.txt

 ⇒ Ta dễ dàng đọc được plaintext của file này, nôm na là:

TFTP không mã hóa lưu lượng truy cập, vì vậy chúng ta phải ngụy trang việc truyền cờ của mình. Hãy tìm cách ẩn cờ, và tôi sẽ kiểm tra lại để xem plan.

- Nội dung này hướng ta đọc tiếp file plan, tiếp tục mở file plan ra để nghiên cứu:
    - Ta xem được nội dung như sau:
        
        ```php
        VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
        ```
        
        Hình 4. Ciphertext của file plan
        
    - Tiếp tục dùng công cụ ROT13 decoder để decrypt đoạn mã này, ta sẽ thu được plaintext như sau:
        
        ```php
              IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
        ```
        
    
        Hình 5. Plaintext của file plan
    
    - Nội dung được dịch nôm na sẽ là: "USE THE PROGRAM AND HIDE IT WITH-DUEDILIGENCE. CHECK OUT THE PHOTOS" - (Sử dụng chương trình và ẩn nó với - CẨN TRỌNG ĐÚNG MỨC. Kiểm tra các bức ảnh).
    
    ⇒ Khả năng cao là một trong các bức ảnh đã được ẩn data (Flag) bằng một passphrase (DUEDILIGENCE) bằng một phương thức nào đó, ở đây là công cụ steghide (ẩn dữ liệu bí mật)
    
- Tại sao lại đoán ra được điều này?
    - Ta tiếp tục khai thác nốt file program.deb để hiểu rõ hơn:
        - Ta sẽ thấy có một file mô tả về công cụ steghide ở bên trong:
        
       ![image](https://hackmd.io/_uploads/S1Bbi1sbZx.png)

        
        Hình 6. File bên trong program.deb
        
        - Vậy ta sẽ dùng steghide để extract từng ảnh ra, với passphrase: DUEDILIGENCE
        
       ![image](https://hackmd.io/_uploads/SyQQjJoW-x.png)

        
        Hình 7. Extract từng file ảnh bmp bằng steghide
        
        - Ta đã tìm thấy file txt ẩn bên trong file ảnh bmp rùi -.-, giờ mở file và lấy flag thui!!!!
