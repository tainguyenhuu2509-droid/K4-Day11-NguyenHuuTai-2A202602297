# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `f5c99557cd52900d5195bf2efecc8f9f920e5516111b06dfcd4490f49898c7aa`; slice `B1-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_006840.jpg, adasind_036720.jpg, adasind_056040.jpg. Frame thiếu trong export: không.
TP=19; FP=2; FN=1; số lần đối chiếu=21; mean IoU của TP=0.804.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.905 | 0.964 | 0.952 |
| precision | 0.905 | 0.887 | 0.750 |
| recall | 0.950 | 0.964 | 0.857 |
| jaccard | 0.864 | 0.852 | 0.750 |
| dice | 0.927 | 0.917 | 0.857 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 6 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Car | 3 | 1 | 0 | 0.952 | 0.750 | 1.000 | 0.750 | 0.857 |
| Pedestrian | 4 | 1 | 0 | 0.952 | 0.800 | 1.000 | 0.800 | 0.889 |
| ThreeWheeler | 6 | 0 | 1 | 0.952 | 1.000 | 0.857 | 0.857 | 0.923 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_006840.jpg | 8 | 1 | 1 | 0.889 | 0.889 | 0.889 |
| adasind_036720.jpg | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_056040.jpg | 7 | 1 | 0 | 0.875 | 0.875 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | <missing> |
|---|---:|---:|---:|---:|---:|
| Bike | 6 | 0 | 0 | 0 | 0 |
| Car | 0 | 3 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 4 | 0 | 0 |
| ThreeWheeler | 0 | 1 | 0 | 6 | 0 |
| <extra> | 0 | 0 | 1 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
