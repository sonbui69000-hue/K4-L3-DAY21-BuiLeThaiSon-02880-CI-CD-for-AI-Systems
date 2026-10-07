# Báo Cáo Lab Day 21 - CI/CD cho AI Systems



| | |
|---|---|
| Họ và tên | Bui Le Thai Son |
| MSSV | 02880 |
| Lớp / Khóa | K4 |
| Repo GitHub | https://github.com/sonbui69000-hue/K4-L3-DAY21-CI-CD-for-AI-Systems |
| Ngày nộp | 2026-10-07  |

---

## 1. Bộ Siêu Tham Số Đã Chọn và Lý Do

| Lần chạy | n_estimators | learning_rate | max_depth | f1_score | accuracy |
|---|---|---|---|---|---|
| 1 | 100 | 0.1 | 3 | 0.7109 | 0.878 |
| 2 | 50 | 0.05 | 2 | 0.6051 | 0.846 |
| 3 | 200 | 0.1 | 5 | 0.7149 | 0.874 |

**Bộ siêu tham số đã chọn:** `n_estimators=200`, `learning_rate=0.1`, `max_depth=5`.

**Lý do:** Run 3 was selected because it achieved the highest F1 score at 0.7149. Run 1 had the highest accuracy at 0.878, but its F1 score was slightly lower. This shows accuracy alone can hide weaker positive-class performance. Lower learning rates may need more estimators to maintain model strength.



---

## 2. Vì Sao Ngưỡng Chất Lượng Đặt Trên F1 Chứ Không Phải Accuracy

The dataset is imbalanced, with only 24.8% of samples in the positive class for income above 50K. A model that always predicts low income can reach about 0.752 accuracy because it follows the majority class. This score looks acceptable but it completely misses high-income cases. Positive-class F1 combines precision and recall. It rewards a model that finds high-income cases while limiting incorrect positive predictions. Accuracy does not show this balance. I used `f1_score(y_eval, preds)` without an averaging option because the lab measures performance for target 1 specifically. Weighted F1 would give more influence to the majority class. Macro F1 would average both classes and hide the required positive-class result. The F1 threshold therefore provides a clearer quality gate for this imbalanced classification task.



---

## 3. Khó Khăn Gặp Phải và Cách Giải Quyết

| Khó khăn | Nguyên nhân | Cách giải quyết |
|---|---|---|
| MLflow tests failed in CI | A partial `mlruns` folder lacked metadata | Tests used an isolated temporary MLflow store |
| EC2 service could not access S3 | The instance had no attached IAM role | An EC2 role with S3 read access was attached |
| Model loading failed on EC2 | Scikit-learn versions did not match | Scikit-learn 1.4.2 was installed on the VM |

---

## 4. So Sánh Bước 2 và Bước 3 (bắt buộc, 2 - 3 câu)

| | f1_score | accuracy |
|---|---|---|
| Bước 2 (chỉ `train_batch1`) | 0.7149 | 0.874 |
| Bước 3 (thêm `train_batch2`) | 0.7354 | 0.882 |

**Nhận xét:** Adding `train_batch2` improved F1 by 0.0205 and accuracy by 0.008. The new data improved performance on the same holdout set.



---

## 5. Phần Bonus Đã Thực Hiện (nếu có)

- [ ] Bonus 1 - Tracking MLflow từ xa với DagsHub: ___
- [ ] Bonus 2 - Điều chỉnh ngưỡng quyết định: ___
- [ ] Bonus 3 - Báo cáo precision / recall tự động: ___
- [ ] Bonus 4 - Hoàn trả về phiên bản trước: ___
- [ ] Bonus 5 - Cảnh báo lệch lạc dữ liệu: ___
