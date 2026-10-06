# Bài 3: Trích xuất Commit Linh hoạt với Git Cherry-Pick

## Mục tiêu
- Hiểu và áp dụng lệnh `git cherry-pick` để trích xuất một hoặc nhiều commit cụ thể từ nhánh này sang nhánh khác.
- Tránh việc phải merge toàn bộ nhánh khi chỉ cần một phần bản sửa lỗi hoặc tính năng nhỏ.

---

## 1. Bối cảnh & Các bước thực hiện

### Bước 1: Giả lập nhánh phát triển `feature/experimental`
Tạo và thực hiện 3 commit trên nhánh `feature/experimental`:
1. Commit 1: `echo "exp 1" > exp1.txt` -> Commit: `feat: experimental feature 1`
2. Commit 2: `echo "bug fix critical" > fix.js` -> Commit: `fix: critical bug fix in core module` (Mã commit: `c7a8b9c`)
3. Commit 3: `echo "exp 2" > exp2.txt` -> Commit: `feat: experimental feature 2`

Lịch sử trên `feature/experimental`:
```text
c7a8b9c fix: critical bug fix in core module
b6a7f8e feat: experimental feature 1
```

### Bước 2: Trích xuất bản vá lỗi về nhánh `main` bằng `git cherry-pick`
Chuyển về nhánh `main`:
```bash
git checkout main
```

Thực hiện cherry-pick duy nhất commit bản vá lỗi `c7a8b9c`:
```bash
git cherry-pick c7a8b9c
```

---

## 2. Kiểm tra Kết quả Lịch sử Git trên `main`

```bash
git log --oneline
```

**Màn hình xuất kết quả `git log`:**
```text
d9e8f7a (HEAD -> main) fix: critical bug fix in core module
9f8e7d6 Initial commit on main
```

File `fix.js` đã được tích hợp vào `main` một cách an toàn mà không đưa các thử nghiệm dở dang (`exp1.txt`, `exp2.txt`) vào sản phẩm.

---

## 3. Kết luận
- `git cherry-pick` là công cụ mạnh mẽ giúp bóc tách đúng những thay đổi cần thiết giữa các nhánh độc lập.
- Giúp giảm thiểu rủi ro khi đưa code lên nhánh sản xuất `main` hoặc `production`.
