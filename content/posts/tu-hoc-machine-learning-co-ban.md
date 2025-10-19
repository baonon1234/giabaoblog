---
title: "Tự học Machine Learning cơ bản: từ số liệu đến mô hình"
date: 2025-10-17
draft: false
tags: ["machine learning", "python", "data"]
categories: ["Học tập"]
cover:
  image: "https://images.unsplash.com/photo-1555949963-aa79dcee981d?q=80&w=1600&auto=format&fit=crop"
  alt: "Machine Learning"
  caption: "Pipeline dữ liệu đến mô hình"
  hidden: false
agent: "viper"
---

Machine Learning nghe có vẻ đồ sộ, nhưng bước chân đầu tiên lại rất cụ thể: chuẩn hóa dữ liệu, chọn mô hình phù hợp và đo lường đúng cách. Nếu bạn là sinh viên IT, hãy bắt đầu với Python, NumPy, Pandas và scikit‑learn. Mục tiêu là hiểu pipeline điển hình: đọc dữ liệu, xử lý thiếu/ngoại lệ, tách tập train/test, huấn luyện, đánh giá và lưu mô hình để dùng lại.

Về dữ liệu, hãy làm quen với thao tác trên DataFrame: lọc, gộp, chuyển kiểu và tạo đặc trưng mới. Tránh rò rỉ dữ liệu bằng cách fit các phép biến đổi (như chuẩn hóa) chỉ trên tập train, rồi áp dụng cho test. Khi dữ liệu lệch phân phối, hãy dùng `StratifiedKFold` hoặc cân bằng lại nhãn. Một thói quen tốt là vẽ biểu đồ nhanh bằng matplotlib/seaborn để phát hiện outlier hoặc mối quan hệ đơn giản.

Về mô hình, bắt đầu từ Logistic Regression, Decision Tree, Random Forest, rồi đến Gradient Boosting. Đừng vội chạy deep learning nếu bài toán và dữ liệu không yêu cầu. Hãy xây `Pipeline` để gói toàn bộ bước tiền xử lý + mô hình, sau đó dùng `GridSearchCV` hoặc `RandomizedSearchCV` tìm siêu tham số. Với bài toán hồi quy, đánh giá bằng MAE/MSE; với phân loại, xem thêm Precision/Recall/AUC thay vì chỉ Accuracy.

Cuối cùng, nghĩ đến triển khai sớm: lưu mô hình bằng `joblib`, viết một API dự đoán nhỏ bằng FastAPI/Flask, và đóng gói bằng Docker. Việc đem mô hình vào một ứng dụng thật, dù nhỏ, sẽ giúp bạn hiểu rõ hơn về hiệu năng, log, và kiểm soát phiên bản dữ liệu. Khi bạn có thể trả lời: mô hình này phục vụ ai, được cập nhật thế nào, và đo lường ra sao, bạn đã đi được rất xa trong hành trình tự học ML.


