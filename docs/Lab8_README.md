# LAB 8 – IMPLEMENTATION & TESTING  
**Môn:** Nhập môn Công nghệ Phần mềm  
**Đề tài:** Quản lý Bán hàng  

---

##  Mục tiêu Lab
Hiểu và thực hành quy trình **cài đặt – kiểm thử – đóng gói – báo cáo chất lượng phần mềm**, bao gồm:
- Xây dựng Unit Test & Load Test.
- Ghi nhận và quản lý lỗi bằng Mentis/Jira/Redmine.
- Đóng gói phần mềm thành file cài đặt `.msi`.
- Viết báo cáo tổng hợp kết quả kiểm thử.

---

##  Cấu trúc thư mục
```
Lab8/
│
├─ README.md
├─ tests/
│   ├─ UnitTests_Login.cs
│   ├─ UnitTests_Product.cs
│   ├─ LoadTest_Result.png
│   └─ Test_Report_Summary.docx
│
├─ defects/
│   ├─ Defect_Report_GroupXX.xlsx
│   └─ Screenshot_Jira.png
│
├─ deployment/
│   ├─ setup.msi
│   ├─ install_step_1.png
│   ├─ install_step_2.png
│   ├─ install_step_3.png
│   └─ Deployment_Guide.docx
│
└─ reports/
    ├─ Testing_Report_GroupXX.docx
    └─ Slides_Testing_Short.pptx
```

---

##  Nội dung chi tiết

### **8.1 – Kiểm thử đơn vị và kiểm thử tải**

####  Yêu cầu
1. **Tạo Unit Test:**
   - Mở Visual Studio → Test → New Test → Unit Test.  
   - Viết test cho ít nhất **2 hàm** (ví dụ: *đăng nhập hệ thống*, *thêm sản phẩm vào danh sách*).  
   - Ghi nhận kết quả: **Pass / Fail**.  
2. **Tạo Load Test:**
   - Tạo kịch bản giả lập **10 – 50 người dùng**.  
   - Ghi nhận **Response Time**, **Error Rate**.  
3. **Xuất báo cáo kết quả kiểm thử** từ Visual Studio.  

---

#### 1. Hai hàm Unit Test (C# – MSTest)

**File:** `UnitTests_Login.cs`
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using SalesManagementSystem.Services;

namespace SalesManagementSystem.Tests
{
    [TestClass]
    public class UnitTests_Login
    {
        [TestMethod]
        public void Login_WithValidCredentials_ShouldReturnTrue()
        {
            var result = AuthService.Login("admin", "123456");
            Assert.IsTrue(result);
        }

        [TestMethod]
        public void Login_WithInvalidPassword_ShouldReturnFalse()
        {
            var result = AuthService.Login("admin", "wrongpass");
            Assert.IsFalse(result);
        }
    }
}

---![alt text](image.png)
BÁO CÁO KẾT QUẢ KIỂM THỬ – LAB 8.1

1. Mục tiêu

Kiểm thử 2 chức năng chính: Đăng nhập và Thêm sản phẩm.

Đánh giá hiệu năng hệ thống khi có nhiều user đồng thời.

2. Kịch bản

Unit Test: 4 test case.

Load Test: 30 user truy cập trong 2 phút với 2 chức năng chính /login và /create-order.

3. Kết quả

Test Case	Mong đợi	Thực tế	Đánh giá
Login hợp lệ	Đăng nhập thành công	Đúng	PASS
Login sai mật khẩu	Báo lỗi sai mật khẩu	Đúng	PASS
Thêm sản phẩm hợp lệ	Lưu thành công	Đúng	PASS
Thêm sản phẩm giá âm	Từ chối lưu	Lỗi chưa xử lý	FAIL

Pass: 3 / 4 (75%)

Fail: 1 / 4 (25%)

Kết quả Load Test:

Avg Response Time: 220 ms

Max: 610 ms

Error Rate: 3%

4. Nhận xét

Ứng dụng chạy ổn định, tốc độ phản hồi tốt (<1s).

Cần bổ sung kiểm tra giá âm sản phẩm.

Đề xuất thêm lớp ValidationService ở tầng Business.

Sinh viên thực hiện:
Trần Thanh Hoàng 

### **8.2 – Quản lý lỗi (Defect Management)**
- Sử dụng **Mentis / Jira / Redmine** để ghi nhận lỗi:
  - ID, mô tả, mức độ, người xử lý, trạng thái.  
- Chu kỳ xử lý: `Open → In Progress → Fixed → Verified → Closed`.  
- **Deliverables:**
  - File `Defect_Report_GroupXX.xlsx`
  - Ảnh chụp màn hình danh sách lỗi.
  c:\Users\Admin\Downloads\Defect_Report_GroupXX.xlsx
  ![alt text](image-1.png)

---

### **8.4 – Đóng gói & Triển khai (Setup Project)**
- Mở Visual Studio → `File → New Project → Setup Project`
- Thêm: `.exe`, `.dll`, `.config`, `.mdf`
- Cấu hình:
  - Tên: *SalesManagementSystem*
  - Version: *1.0.0*
  - Publisher: *GroupXX*
  - Shortcut Desktop + Start Menu.
- **Deliverables:**
  - File `.msi`
  - Ảnh chụp quá trình cài đặt
  - File `Deployment_Guide.docx`
  Quy trình thực hiện (mô phỏng setup thực tế)

**Bước 1. Tạo dự án Setup**
- Visual Studio → `File > New Project > Setup Project`
- Project name: `SalesManagementSystem_Setup`
- Output type: `.msi`

**Bước 2. Thêm các thành phần vào bộ cài**
- `SalesManagementSystem.exe` (ứng dụng chính)
- `SalesManagementSystem.dll` (business logic / service layer)
- `app.config` (chuỗi kết nối DB, cấu hình ban đầu)
- `database.mdf` (hoặc script khởi tạo DB)
- Thêm các dependency khác nếu có (ví dụ .NET runtime)

**Bước 3. Cấu hình gói cài đặt**
- Product Name: `Sales Management System`
- Version: `1.0.0`
- Publisher: `Trần Thanh Hoàng`
- Default install folder:
  `C:\Program Files\SalesManagementSystem\`
- Add Shortcut:
  - Desktop shortcut → trỏ tới `SalesManagementSystem.exe`
  - Start Menu shortcut → thư mục `Sales Management System`

**Bước 4. Build bộ cài**
- Build Solution → sinh ra file:
  `SalesManagementSystem_Setup.msi`

**Bước 5. Cài thử trên máy khác**
Chạy file `.msi` vừa tạo và chụp lại từng màn hình:

- Màn hình 1: “Welcome to the Sales Management System Setup Wizard”
- Màn hình 2: Chọn thư mục đích (Destination Folder)
- Màn hình 3: Installing…
- Màn hình 4: “Setup Completed Successfully” + nút Finish
- Màn hình 5: Ứng dụng mở được, có icon ngoài Desktop
Báo cáo ngắn (Deployment_Guide.docx)

**Tên file:** `deployment/Deployment_Guide.docx`

**Nội dung báo cáo mẫu:**

**1. Mục tiêu**
Hướng dẫn người dùng cuối (end user) cài đặt và chạy ứng dụng “Sales Management System” mà không cần môi trường lập trình.

**2. Yêu cầu hệ thống**
- HĐH: Windows 10/11 (64-bit)
- .NET Runtime đã cài đặt (phiên bản tương ứng với app)
- Quyền cài đặt (Run as Administrator nếu cần)

**3. Quy trình cài đặt**
Bước 1: Double-click `SalesManagementSystem_Setup.msi`  
Bước 2: Chọn thư mục cài đặt hoặc giữ mặc định  
Bước 3: Nhấn “Install” và chờ quá trình copy file  
Bước 4: Nhấn “Finish” để kết thúc  
Bước 5: Kiểm tra icon ngoài Desktop hoặc Start Menu

**4. Kiểm tra sau cài đặt**
- Mở ứng dụng → màn hình đăng nhập xuất hiện
- Đăng nhập thử bằng tài khoản test (`admin / 123456`)
- Mở chức năng “Quản lý sản phẩm” → không lỗi
- Xác nhận database `.mdf` đã kết nối thành công

**5. Nhận xét**
- Cài đặt thành công trên máy khác (máy không có source code).
- Shortcut ngoài Desktop hoạt động bình thường.
- Ứng dụng khởi động được bằng .msi build ra từ Setup Project.
c:\Users\Admin\Downloads\SalesManagementSystem_Setup.exe
c:\Users\Admin\Downloads\install_step_1.png
c:\Users\Admin\Downloads\install_step_2.png
c:\Users\Admin\Downloads\install_step_3.png
c:\Users\Admin\Downloads\Deployment_Guide.docx


---

---

### **8.4 – Báo cáo kiểm thử tổng hợp**
- Viết `Testing_Report_GroupXX.docx` gồm:
  1. Mục tiêu kiểm thử  
  2. Danh sách Test Case (ID, Input, Expected, Actual, Result)  
  3. Danh sách lỗi (Defect Log)  
  4. Thống kê Pass/Fail  
  5. Đánh giá và đề xuất cải tiến  
- **Deliverables:**
  - File Word báo cáo
  - Slide 3–4 trang trình bày tóm tắt
  báo cáo
  c:\Users\Admin\Downloads\Testing_Report_Group01.docx
  tóm tắt
  c:\Users\Admin\Downloads\Slides_Testing_Short.pptx
  Báo cáo tổng hợp mẫu (Testing_Report_Group01.docx)

**1. Mục tiêu kiểm thử**  
Đảm bảo hệ thống Quản lý Bán hàng hoạt động đúng yêu cầu nghiệp vụ, có độ ổn định và khả năng xử lý lỗi đầu vào tốt.  
Kiểm tra các chức năng chính: Đăng nhập – Quản lý sản phẩm – Quản lý đơn hàng.

**2. Danh sách Test Case**

| ID | Mô tả | Input | Expected | Actual | Result |
|----|--------|--------|-----------|---------|---------|
| TC01 | Đăng nhập hợp lệ | admin / 123456 | Thành công | Đúng | Pass |
| TC02 | Đăng nhập sai mật khẩu | admin / sai | Báo lỗi | Đúng | Pass |
| TC03 | Thêm sản phẩm giá âm | -10000 | Báo lỗi | Vẫn lưu | Fail |
| TC04 | Tạo đơn hàng khi tồn kho = 0 | SL = 1 | Báo hết hàng | Vẫn tạo | Fail |
| TC05 | Thêm sản phẩm hợp lệ | Giá = 850000 | Lưu thành công | Đúng | Pass |

**3. Danh sách lỗi (Defect Log)**  

| Bug ID | Module | Mô tả lỗi | Severity | Status |
|---------|----------|------------------------|-----------|-----------|
| BUG-01 | Login | Lỗi 500 khi nhập sai mật khẩu | High | Fixed |
| BUG-02 | Product | Cho phép nhập giá âm | High | Open |
| BUG-03 | Order | Vẫn tạo được đơn khi hết hàng | Medium | In Progress |

**4. Thống kê kết quả kiểm thử**  
- Tổng số test: 5  
- Pass: 3  
- Fail: 2  
- Tỷ lệ thành công: **60%**  

**5. Nhận xét & Đề xuất**  
- Hệ thống đạt ổn định trung bình, giao diện hoạt động tốt.  
- Cần bổ sung kiểm tra dữ liệu đầu vào cho Product và Order.  
- Đề xuất fix toàn bộ lỗi trước khi triển khai bản chính thức.

**6. Kết luận chất lượng phần mềm**  
- Ứng dụng có thể vận hành ở môi trường test (staging).  
- Sau khi xử lý lỗi tồn tại, có thể bàn giao để triển khai thật.

---

---

## Thành viên thực hiện
- Trần Thanh Hoàng  
 

---

## ⚙️ Hướng dẫn chạy thử
1. Mở Visual Studio → Open Solution chứa source.  
2. Chạy `UnitTests_Login` & `UnitTests_Product` để xem kết quả kiểm thử.  
3. Xem ảnh và báo cáo trong thư mục `/tests` và `/reports`.  
4. Mở thư mục `/deployment` → chạy `setup.msi` để kiểm tra cài đặt.  

---

## 🏁 Ghi chú
Repo phục vụ cho mục đích học tập và nộp **Lab 8 – Nhập môn Công nghệ Phần mềm**.  
Mọi file `.docx`, `.xlsx`, `.pptx` là minh họa cho cấu trúc chuẩn yêu cầu trong đề bài.


---

## 🎯 KẾT QUẢ CẦN ĐẠT

### 🔹 Lab 8.1 – Kiểm thử đơn vị và kiểm thử tải
- Hiểu và áp dụng được quy trình **kiểm thử đơn vị (Unit Test)** trong môi trường Visual Studio.
- Biết tạo, chạy và ghi nhận **kết quả Pass/Fail** cho từng test case cụ thể.
- Biết xây dựng **Load Test mô phỏng nhiều người dùng**, đo được Response Time và Error Rate.
- Xuất được **báo cáo kết quả kiểm thử (Testing Report)** đúng định dạng.

### 🔹 Lab 8.2 – Quản lý lỗi bằng Mentis / Jira / Redmine
- Sử dụng được công cụ quản lý lỗi (bug tracking tool).
- Tạo, phân loại và theo dõi **chu kỳ sống của lỗi (Defect Life Cycle)**.
- Ghi nhận **tối thiểu 3 lỗi thực tế**, cập nhật trạng thái xử lý.
- Hoàn thiện **báo cáo tổng hợp lỗi (Defect Report)** theo mẫu.

### 🔹 Lab 8.4 – Đóng gói và triển khai phần mềm
- Tạo được **project đóng gói (Setup Project)** trong Visual Studio.
- Tùy chỉnh thông tin cài đặt: tên, version, icon, publisher.
- Sinh ra được **file cài đặt `.msi` / `.exe`** chạy được trên máy khác.
- Thực hiện **cài đặt thành công** và chụp lại các bước cài.

### 🔹 Lab 8.4 – Báo cáo kiểm thử tổng hợp
- Biết lập **Testing Report tổng hợp** gồm:
  - Danh sách Test Case có kết quả thực tế.
  - Danh sách lỗi kèm trạng thái xử lý.
  - Thống kê tỷ lệ Pass/Fail (%).
- Viết được **nhận xét và đánh giá chất lượng phần mềm**, đưa ra **đề xuất cải tiến**.
- Chuẩn bị được **slide báo cáo tóm tắt (3–4 trang)** trình bày trong buổi bảo vệ lab.

---

> ✅ *Sau khi hoàn thành đầy đủ 4 nội dung, sinh viên nắm vững quy trình kiểm thử phần mềm:  
từ tạo test case → phát hiện lỗi → đóng gói sản phẩm → báo cáo kết quả chất lượng hệ thống.*
