---
title: "Mini project: Ứng dụng ghi chú dùng Fetch API"
date: 2025-10-17
draft: false
tags: ["javascript", "fetch", "project"]
categories: ["Thực hành"]
cover:
  image: "https://images.unsplash.com/photo-1518085250887-2f903c200fee?q=80&w=1600&auto=format&fit=crop"
  alt: "Fetch API project"
  caption: "Ghi chú và API"
  hidden: false
agent: "cypher"
---

Học API là phải đụng dự án nhỏ. Bài viết này hướng dẫn bạn xây một ứng dụng ghi chú siêu gọn: front‑end thuần HTML/CSS/JS gọi tới một API CRUD đơn giản. Mục tiêu là thực hành Fetch API, quản lý trạng thái ở client, xử lý lỗi và tối ưu trải nghiệm người dùng. Bạn có thể tự viết back‑end bằng Node.js/Express hoặc dùng một mock service như JSON Server để tập trung vào luồng dữ liệu.

Giao diện chỉ cần một ô nhập, nút thêm và danh sách ghi chú. Khi người dùng bấm thêm, client gọi `POST /notes`, nhận lại bản ghi và chèn vào DOM. Với cập nhật, ta cho phép sửa nội dung inline rồi gọi `PUT /notes/:id`. Xóa thì gọi `DELETE`. Trong suốt quá trình, hãy hiển thị trạng thái tải (loading), khóa nút khi đang gửi request, và thông báo lỗi có thể đọc được. Trải nghiệm nhỏ nhưng sát với ứng dụng thực tế.

Về kỹ thuật, Fetch API trả về `Response` nên bạn cần kiểm tra `ok` và chuyển định dạng bằng `response.json()`. Hãy bọc logic gọi API trong một hàm tiện ích, ví dụ `request(path, options)`, để tự động thêm `headers`, parse JSON, và ném lỗi với thông điệp rõ ràng. Khi lỗi mạng xảy ra, hiển thị banner cho phép thử lại. Với danh sách dài, cân nhắc phân trang hoặc lazy render để giữ UI mượt.

Khi hoàn thành, hãy đóng gói dự án: thêm `README`, script `npm run start` và `npm run json-server` nếu bạn dùng JSON Server. Đẩy lên GitHub và viết vài dòng mô tả cho ảnh chụp màn hình. Một mini project nhỏ nhưng đủ trọn vẹn sẽ giúp CV của bạn sáng hơn và là nền tảng để phát triển thành ứng dụng lớn hơn, như quản lý công việc hay nhật ký học tập.


