# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Một vạch nằm ở khu vực bên trái, khoảng giữa chiều cao ảnh, kéo theo hướng gần thẳng đứng và hơi cong theo phối cảnh. Vạch còn lại nằm ở khu vực tiền cảnh phía dưới bên phải, là phần vạch trắng chạy chéo từ vùng giữa ảnh xuống phía dưới và tiếp tục về phía mép phải. Các polyline được đặt theo phần vạch sơn thực tế còn nhìn thấy trong frame.

- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Các đoạn vạch sơn khác xuất hiện ở khu vực giữa và xa của bãi xe không được gán nhãn khi đoạn vạch không đủ rõ hoặc khó xác định chắc chắn đây là đường phân chia ô đỗ. Ngoài ra, không gán các biên của mặt đường hoặc các dấu sơn không có chức năng phân chia ô đỗ.

- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon `free_space` bao phần mặt đường trống có thể quan sát được ở khu vực tiền cảnh, giới hạn bởi các vạch sơn của khu vực đỗ xe và mép khung hình. Polygon dừng tại các vị trí mà biên của vùng trống không còn quan sát rõ. Một phần vùng gần mép dưới ảnh bị giới hạn bởi chính biên khung hình; không có đối tượng lớn che khuất phần trung tâm của polygon.

- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): không có.