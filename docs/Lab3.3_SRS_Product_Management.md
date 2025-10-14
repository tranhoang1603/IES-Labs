# PTIT – LAB 3.3: Software Requirement Specification (SRS) – Product Management

## 1. Introduction
- **Purpose:** Mô tả yêu cầu hệ thống quản lý sản phẩm.
- **Scope:** CRUD sản phẩm, tìm kiếm, phân trang, sắp xếp.
- **References:** IEEE 830; tài liệu môn học.

## 2. Overall Description
- Kiến trúc 3-tier; DB quan hệ H2/MySQL.  
- Người dùng: Admin/Staff.  
- Ràng buộc: thời gian lab, dữ liệu demo.

## 3. Specific Requirements

### 3.1 Functional Requirements
- FR1: Tạo sản phẩm (validate name, sku unique, price>=0, quantity>=0).
- FR2: Danh sách + phân trang + sắp xếp theo giá.
- FR3: Xem chi tiết.
- FR4: Cập nhật.
- FR5: Xóa.
- FR6: Tìm kiếm theo tên/SKU.
- FR7 (tùy chọn): Đổi trạng thái ACTIVE/INACTIVE.

### 3.2 Non-functional Requirements
- Hiệu năng: 10 bản ghi < 500ms (máy lab).
- Bảo mật: validate input; ẩn stacktrace; CORS phù hợp.
- Khả chuyển: H2 ↔ MySQL qua cấu hình.
- Bảo trì: tách Controller/Service/Repository.

### 3.3 Interface Requirements
- REST/JSON; DB bảng products.
