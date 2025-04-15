# BaiTap4
# Nguyễn Phương Nam K58KTP
yêu cầu bài toán:
Tạo csdl cho hệ thống TKB (đã nghe giảng, đã xem cách làm)
Nguồn dữ liệu: TMS.tnut.edu.vn
Tạo các bảng tuỳ ý (3nf)
Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra. trả lời câu hỏi: trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.
các bước thực hiện:

Tạo github repo mới: đặt tên tuỳ ý (có liên quan đến bài tập này)
tạo file readme.md, edit online nó: paste những ảnh chụp màn hình gõ text mô tả cho ảnh đóm
Gợi ý: sử dung tms => dữ liệu thô => tiền xử lý => dữ liệu như ý (3nf) tạo các bảng với struct phù hợp insert nhiều rows từ excel vào cửa sổ edit dữ liệu 1 table (quan sát thì sẽ làm đc) deadline: 15/4/2025
Lọc giáo viên trong nguồn dữ liệu: TMS.tnut.edu.
# Tạo Databases mới :
![image](https://github.com/user-attachments/assets/168001cd-be89-42b2-b93d-8735b67b2ea3)
Tạo bảng GiaoVien :
![image](https://github.com/user-attachments/assets/01556c80-a16f-459c-8369-48b33b8a38a9)
Tạo bảng LichDay :
![image](https://github.com/user-attachments/assets/0c0ec25d-0e2d-4265-bc7c-c01dbc1e64f7)
Tạo bảng Lop :
![image](https://github.com/user-attachments/assets/7e2fd9a3-757a-4e05-abda-df464b073494)
Tạo bảng MonHoc : 
![image](https://github.com/user-attachments/assets/91d3ce0e-ab9f-4cc6-a12c-0515b723def1)
Tại bảng lớp học, thiết lập khoá ngoại (FK)
Thiết lập khoá ngoại giữa bảng (LichDay) và bảng (GiaoVien) thông qua id
Thiết lập khoá ngoại giữa bảng (LichDay) và bảng (MonHoc) thông qua id
Thiết lập khoá ngoại giữa bảng (LichDay) và bảng (Lop) thông qua id
Các FK bảng LichDay : 
![image](https://github.com/user-attachments/assets/0b0a2b9f-4eb3-4cdd-bb6b-8bd4d4f6e938)

# Thêm thông tin demo cho bảng 
Truy cập Link nguồn dữ liệu TMS.tnut.edu.vn
![image](https://github.com/user-attachments/assets/3b256ae6-b7b0-4e3b-ab80-272c50b4667b)


Điền thông tin bảng GiaoVien  :
![image](https://github.com/user-attachments/assets/89ba6813-2ae0-4270-bdb5-f86388650ff4)


Điền thông tin bảng MonHoc :
![image](https://github.com/user-attachments/assets/b8425f6a-1dcd-4786-9e47-e35bb8bcfa9c)


Điền đầy đủ thông tin bảng LichDay : 
![image](https://github.com/user-attachments/assets/e358c4b2-5d51-4a0f-b64f-29dd905b743c)

Trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.
Sử dụng DECLARE @datetime1 DATETIME = '2025-04-08 06:30:00';
DECLARE @datetime2 DATETIME = '2025-04-08 09:10:00';
Lấy tất cả các buổi học có thời gian giáo viên đang dậy học.
![image](https://github.com/user-attachments/assets/84ab2bd6-d249-49e6-bcfb-3b7829da64bd)
SD SECLECT DISTINCT sẽ giữ lại mỗi dòng dữ liệu duy nhất, tức là nếu hai dòng có tất cả các cột giống hệt nhau, thì chỉ lấy 1 dòng duy nhất.
![image](https://github.com/user-attachments/assets/0f8fc8c6-da51-4773-b6b9-718f39cdf5ef)
SD JOIN các bảng liên quan
![image](https://github.com/user-attachments/assets/aadfa5ac-5194-4f03-af21-463744138fab)
Đây là điều kiện lọc để tìm các tiết học có giao với khoảng thời gian chỉ định.
So sánh Thời gian kết thúc buổi học > thời điểm bắt đầu (@datetime1) AND Thời gian bắt đầu buổi học < thời điểm kết thúc (@datetime2)
![image](https://github.com/user-attachments/assets/d2f6ec3c-0f73-4758-bfd5-d313e21a7e01)
# Ví dụ code truy vấn : 
DECLARE @datetime1 DATETIME = '2025-03-17 15:30:00';
DECLARE @datetime2 DATETIME = '2025-03-19 18:30:00';

SELECT 
    GV.HoTen AS HoTenGV,
    MH.TenMonHoc AS TenMonDay,
    TKB.GioVao,
    TKB.GioRa
FROM TKB
JOIN GIAOVIEN GV ON TKB.MaGV = GV.MaGV
JOIN MONHOC MH ON TKB.MaMH = MH.MaMH
WHERE 
    (TKB.GioVao BETWEEN CAST(@datetime1 AS TIME) AND CAST(@datetime2 AS TIME)) 
    OR
    (TKB.GioRa BETWEEN CAST(@datetime1 AS TIME) AND CAST(@datetime2 AS TIME)) 
    OR
    (CAST(@datetime1 AS TIME) BETWEEN TKB.GioVao AND TKB.GioRa)
    OR
    (CAST(@datetime2 AS TIME) BETWEEN TKB.GioVao AND TKB.GioRa);


