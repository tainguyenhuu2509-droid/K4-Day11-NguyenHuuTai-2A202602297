# Sensor context

- Rig: Dữ liệu có vẻ được thu từ một camera fisheye gắn trên phương tiện, cung cấp trường nhìn rất rộng về khu vực phía trước và xung quanh xe. Từ ảnh chỉ có thể xác định theo quan sát rằng camera được gắn trên xe; không thể xác định chính xác vị trí lắp đặt, hướng lắp đặt hoặc cấu hình rig. Do đó không giả định các thông số rig hay calibration cụ thể khi không có tài liệu đi kèm.

- `ego_body`: Một phần thân của chính phương tiện mang camera có thể nhìn thấy ở khu vực gần mép dưới của một số frame, tạo thành một vùng cong ở phía dưới ảnh. Không thấy rõ và không nhất quán các chi tiết như vô lăng hoặc gương chiếu hậu. Phần thân xe không xuất hiện trong tất cả các frame.

- Lens circle: Vòng kính fisheye nằm xấp xỉ ở trung tâm ảnh và chiếm phần lớn khung hình. Trường nhìn dạng tròn mở rộng gần tới các biên ảnh, với một số vùng tối ở phía ngoài các góc/mép ảnh. Hiện tượng méo hình fisheye rõ hơn khi tiến về phía biên của vòng kính.

- Giới hạn của camera: Các annotation chỉ dựa trên góc nhìn của một camera fisheye duy nhất. Không nên suy ra chính xác khoảng cách, độ sâu, tư thế camera, thông số calibration hoặc mối tương ứng với các camera khác chỉ từ hình ảnh. Một phần cảnh hoặc thân xe có thể nằm ngoài trường nhìn hoặc bị che khuất.