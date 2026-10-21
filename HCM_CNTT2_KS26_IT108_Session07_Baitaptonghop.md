**(Bài tập tổng hợp) Terminal Nâng Cao & Tiền Đề Scripting** 

### **Phần 1 – Đọc và định vị tệp tin**

*1.Viết lệnh PowerShell in toàn bộ nội dung file `logs\server.log` ra màn hình.*  
Get-Content .\\logs\\server.log  
 2\. File 500 MB có thể khiến Notepad bị treo vì phải xử lý lượng dữ liệu lớn, tốn RAM. PowerShell cho phép đọc dữ liệu theo từng dòng, nên có thể giảm áp lực bộ nhớ. Tuy nhiên, `Get-Content` vẫn có thể tốn thời gian và tài nguyên nếu xử lý file lớn.   
3\. Tìm tất cả file `.log` trong project:   
Get-ChildItem \-Path . \-Filter \*.log \-File \-Recurse  
**Phần 2 – Lọc dữ liệu và Pipe**

4.Dựa vào nội dung file ở mục 3c, viết lệnh PowerShell lọc ra các dòng chứa chữ `ERROR`.  
Get-Content .\\logs\\server.log | Select-String "ERROR"  
5.Phân biệt bản chất 2 câu hỏi sau, rồi hoàn thành bảng:

| Câu hỏi | Lệnh PowerShell |
| ----- | ----- |
| *"File* `server.log` *đang nằm ở thư mục nào?"* | Get-ChildItem \-Path . \-Filter server.log \-File \-Recurse  |
| *"Bên trong file này có dòng nào chứa lỗi 404 không?"* | Select-String \-Path .\\logs\\server.log \-Pattern "404"  |

**Pipe dùng để chuyển kết quả của lệnh trước sang lệnh sau để xử lý tiếp:**  
Get-ChildItem \-Path . \-Filter "\*.log" \-File \-Recurse | Select-String \-Pattern "404"

6\. Viết **một lệnh PowerShell duy nhất** để: tìm đệ quy tất cả file `.log` trong `devops-lab`, rồi trong các file đó lọc ra những dòng **không chứa** chữ `INFO`.

Get-ChildItem \-Path . \-Filter \*.log \-File \-Recurse | Select-String \-NotMatch "INFO"

**Phần 3 – Toán tử chuyển hướng `>` và `>>`**

7.Viết lệnh hoàn chỉnh: đọc `logs\server.log`, lọc các dòng chứa `404`, và **ghi đè** kết quả vào `reports\daily_report.txt`.

Get-Content .\\logs\\server.log | Select-String "404" \> .\\reports\\daily\_report.txt

8\. 30 phút sau hệ thống phát sinh thêm một lỗi 404 mới.

+ Viết lệnh **thêm dòng lỗi mới vào cuối** `reports\daily_report.txt` mà **không làm mất** báo cáo cũ ở Câu 7\.

 Get-Content .\\logs\\server.log | Select-String "404" \>\> .\\reports\\daily\_report.txt

        \+Giải thích ngắn gọn điều gì xảy ra với báo cáo cũ nếu bạn lỡ tay dùng nhầm toán tử còn lại.

   `>>`: Ghi nối vào cuối file.  
        Nếu dùng nhầm `>`: Nội dung báo cáo cũ sẽ bị ghi đè.

### **Phần 4 – Shell Script và lệnh `echo`**

9\. Viết lệnh dùng `Write-Output` (hoặc `echo`) để:

Write-Output "Dang loc log..."

"=== LOG REPORT \===" \> .\\reports\\daily\_report.txt

10\. Viết toàn bộ nội dung file `scripts\auto_report.ps1`, đóng gói quy trình của Câu 7 và Câu 9, gồm đủ:

\# Loc loi 404 tu server.log va ghi vao bao cao

Write-Output "Dang loc log..."

"=== LOG REPORT \===" \> .\\reports\\daily\_report.txt

Get-Content .\\logs\\server.log | Select-String "404" \>\> .\\reports\\daily\_report.txt

### **Phần 5 – Viết, chạy và thẩm định Script AI**

11.Trình bày đúng **Quy trình 3 bước thực thi Script** cho file `scripts\auto_report.ps1` ở Câu 10:

Bước 1 – Viết: Lưu nội dung vào file `scripts\auto_report.ps1`.

Bước 2 – Chạy: Tại thư mục gốc `devops-lab`, thực hiện:  
.\\scripts\\auto\_report.ps1

Bước 3 – Kiểm tra: Đọc nội dung báo cáo:   
Get-Content .\\reports\\daily\_report.txt  
12\. Đọc đoạn script AI ở mục 3d, áp dụng **3 Nguyên tắc Vàng thẩm định Script AI**:  
a) Dòng lệnh nguy hiểm   
Remove-Item \-Recurse \-Force C:\\\*  
Lệnh này có thể xóa hàng loạt tệp và thư mục trên ổ `C:`, gây mất dữ liệu hoặc làm Windows không hoạt động bình thường.   
b) Sửa đường dẫn để chỉ tác động vào thư mục tạm của project:   
Remove-Item \-Path .\\temp\\\* \-Recurse \-Force

