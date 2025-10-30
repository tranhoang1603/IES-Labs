# PTIT – LAB CHƯƠNG 7 (Phân tích & Thiết kế Hệ thống)

**Sinh viên:** Trần Thanh HoàngHoàng 
**Lớp:** K23DTCN476
**Đề tài hệ thống:** Quản lý Sinh viên  
**Môn:** Nhập môn Công nghệ Phần mềm  
**GVHD:** 

---

  


---

##  Cấu trúc đề xuất trên GitHub
```text
ptit-lab7/
│
├─ README.md
├─ Lab7.1_ClassDiagram.md
├─ Lab7.2_ActivityDiagram.md
├─ Lab7.4_Component_Deployment.md
├─ Lab7.5_UI_and_Database.md
└─ Lab7.6_MVC_or_SOA.md
```

---

##  LAB 7.1 – BIỂU ĐỒ LỚP (CLASS DIAGRAM)

### Mục tiêu
Xác định các lớp và mối quan hệ trong hệ thống Quản lý Sinh viên, phân loại theo 3 nhóm: Entity, Boundary, Control.

### Mô tả bài toán
Hệ thống Quản lý Sinh viên lưu thông tin sinh viên (mã SV, họ tên, ngày sinh, lớp, khoa). Cho phép thêm, sửa, xóa, tìm kiếm.

### Các lớp chính
- **Entity class:** `SinhVien`, `Lop`, `Khoa`  
- **Boundary class:** `SinhVienForm`  
- **Control class:** `SinhVienController`

### Biểu đồ lớp
![Class Diagram](Lab7.1_ClassDiagram.png)

### Mô tả chức năng lớp
| Lớp | Vai trò | Thuộc tính | Phương thức |
|------|----------|-------------|--------------|
| SinhVien | Lưu dữ liệu sinh viên | maSV, hoTen, ngaySinh, maLop | Them(), Sua(), Xoa(), TimKiem() |
| SinhVienController | Xử lý logic nghiệp vụ | - | LuuSinhVien(), KiemTraDuLieu() |
| SinhVienForm | Giao diện nhập liệu | - | HienThi(), NhanInput() |

---

## LAB 7.2 – BIỂU ĐỒ HOẠT ĐỘNG (ACTIVITY DIAGRAM)

### Use Case được chọn
Đăng nhập hệ thống.

### Flow of Events
**Preconditions:** Người dùng đã có tài khoản.  
**Main Flow:**  
1. Nhập username + password.  
2. Hệ thống kiểm tra thông tin.  
3. Nếu hợp lệ → vào màn hình chính.  
**Alternative Flow:** Sai thông tin → báo lỗi, nhập lại.

### Activity Diagram
![Activity Diagram](Lab7.2_ActivityDiagram.png)

---

##  LAB 7.4 – COMPONENT & DEPLOYMENT DIAGRAM

### Component Diagram
**Các module:**  
- UI Module: Giao diện người dùng.  
- Business Logic Module: Xử lý nghiệp vụ.  
- Data Access Module: Làm việc với CSDL.

**Quan hệ:** UI → Business → Database.

![Component Diagram](Lab7.4_ComponentDiagram.png)

### Deployment Diagram
**Mô hình triển khai:** 3 tầng – Client / App Server / Database Server.  
**Kết nối:** Client ↔ AppServer ↔ SQLServer.

![Deployment Diagram](Lab7.4_DeploymentDiagram.png)

### Mô tả triển khai
Ứng dụng triển khai dạng client-server. Client gửi yêu cầu đến AppServer, AppServer xử lý nghiệp vụ và truy vấn dữ liệu từ SQL Server. Mô hình này giúp mở rộng, dễ bảo trì và tăng tính bảo mật.

---

##  LAB 7.5 – THIẾT KẾ GIAO DIỆN & CƠ SỞ DỮ LIỆU

### Giao diện người dùng (UI)
- **Main Form:** hiển thị menu Quản lý Sinh viên / Lớp / Khoa.
- **CRUD Form:** nhập thông tin sinh viên, các nút Thêm – Sửa – Xóa – Xuất báo cáo.

![Main Form](Lab7.5_UI_Main.png)
![CRUD Form](Lab7.5_UI_CRUD.png)

### Cơ sở dữ liệu (ERD)
**Bảng:**
- Khoa(maKhoa PK, tenKhoa)
- Lop(maLop PK, tenLop, maKhoa FK)
- SinhVien(maSV PK, hoTen, ngaySinh, maLop FK)

![ERD Diagram](Lab7.5_ERD.png)

### Script SQL (database_script.sql)
```sql
CREATE DATABASE QuanLySinhVien;
GO
USE QuanLySinhVien;
GO
CREATE TABLE Khoa (
  maKhoa VARCHAR(10) PRIMARY KEY,
  tenKhoa NVARCHAR(100)
);
CREATE TABLE Lop (
  maLop VARCHAR(10) PRIMARY KEY,
  tenLop NVARCHAR(100),
  maKhoa VARCHAR(10) FOREIGN KEY REFERENCES Khoa(maKhoa)
);
CREATE TABLE SinhVien (
  maSV VARCHAR(10) PRIMARY KEY,
  hoTen NVARCHAR(100),
  ngaySinh DATE,
  maLop VARCHAR(10) FOREIGN KEY REFERENCES Lop(maLop)
);
```

---

##  LAB 7.6 – KIẾN TRÚC PHẦN MỀM (MVC / SOA)

### Mục tiêu
Xây dựng ứng dụng demo nhỏ trong Visual Studio minh họa kiến trúc **MVC**.

### Kiến trúc MVC
**Model:** Lớp `SinhVien` và `SinhVienRepository` quản lý dữ liệu.  
**View:** Form nhập liệu (hoặc Razor View) hiển thị danh sách sinh viên.  
**Controller:** `SinhVienController` nhận yêu cầu từ View, điều phối luồng xử lý.

### Luồng xử lý
1. User nhập thông tin → View gửi đến Controller.  
2. Controller gọi Model để lưu vào DB.  
3. Model xử lý truy vấn → trả kết quả.  
4. Controller gửi phản hồi lại View hiển thị thông báo.

### Ảnh chạy thực tế
![Run App](Lab7.6_RunApp.png)
![Insert Success](Lab7.6_Insert.png)

### Mô tả kiến trúc
Ứng dụng được chia làm 3 tầng riêng biệt giúp dễ dàng bảo trì, test và mở rộng. Giao diện và cơ sở dữ liệu hoàn toàn tách biệt, giao tiếp qua lớp trung gian Controller.

---

##  HƯỚNG DẪN LIÊN KẾT GITHUB

1. Tạo repo trên GitHub: `ptit-lab7`
2. Push file này và ảnh, code liên quan:
```bash
git add .
git commit -m "Upload full PTIT Lab7 package"
git push origin main
```


--

