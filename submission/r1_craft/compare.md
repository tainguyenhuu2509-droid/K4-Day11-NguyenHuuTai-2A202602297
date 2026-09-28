# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_006840.jpg
- L1 center IGNORE_SCOPE
- L2+R4 center WRONG_CLASS
## adasind_036720.jpg
## adasind_056040.jpg
- L3+R2 edge ATTRIBUTE
- L7 mid SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 10 | 9 | 1 | 1 |
| mid | 7 | 7 | 0 | 1 |
| edge | 3 | 3 | 0 | 0 |
