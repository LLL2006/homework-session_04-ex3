# Báo Cáo Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

## 1. Mục tiêu
- Tạo cặp khóa SSH bảo mật sử dụng thuật toán **Ed25519**.
- Thêm Public Key vào tài khoản GitHub cá nhân.
- Cấu hình và kiểm tra kết nối an toàn tới GitHub qua SSH.
- Liên kết Git Repository cục bộ với Remote Repository trên GitHub và đẩy (push) mã nguồn thành công.

---

## 2. Quá trình thực hiện

### Bước 1: Khởi tạo cặp khóa SSH (Ed25519)
Chạy lệnh sau trên terminal/PowerShell (thay `your_email@example.com` bằng email đăng ký GitHub):
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Khi được hỏi nơi lưu file (`Enter file in which to save the key`), nhấn **Enter** để chọn đường dẫn mặc định (`~/.ssh/id_ed25519`).
- Nhập Passphrase bảo vệ (hoặc nhấn **Enter** 2 lần để bỏ qua passphrase).

### Bước 2: Lấy nội dung Public Key và thêm vào GitHub
1. Mở và sao chép nội dung file khóa công khai (`id_ed25519.pub`):
   ```bash
   cat ~/.ssh/id_ed25519.pub
   # Hoặc trên Windows PowerShell:
   Get-Content ~\.ssh\id_ed25519.pub
   ```
2. Truy cập GitHub: **Settings** > **SSH and GPG keys** > **New SSH key**.
3. Điền **Title** (ví dụ: `Laptop-CaNhan`) và dán toàn bộ chuỗi Public Key vào phần **Key**, sau đó nhấn **Add SSH key**.

> [!WARNING]
> Tuyệt đối **không** chia sẻ hoặc tải file Private Key (`id_ed25519`) lên GitHub hay bất kỳ đâu.

### Bước 3: Kiểm tra kết nối SSH tới GitHub
Chạy lệnh kiểm tra kết nối:
```bash
ssh -T git@github.com
```
**Kết quả mong đợi:**
```text
Hi [username]! You've successfully authenticated, but GitHub does not provide shell access.
```

### Bước 4: Khởi tạo Git Repo cục bộ và liên kết Remote Repository bằng SSH
1. Khởi tạo repository cục bộ:
   ```bash
   git init
   git branch -M main
   ```
2. Thêm file và tạo commit đầu tiên:
   ```bash
   git add README.md
   git commit -m "docs: add report for session 4 exercise 3 (SSH config & push to GitHub)"
   ```
3. Liên kết với Remote Repository trên GitHub qua giao thức SSH:
   ```bash
   git remote add origin git@github.com:[your-username]/[your-repo].git
   ```
4. Kiểm tra cấu hình remote URL:
   ```bash
   git remote -v
   ```
   **Kết quả:**
   ```text
   origin  git@github.com:[your-username]/[your-repo].git (fetch)
   origin  git@github.com:[your-username]/[your-repo].git (push)
   ```

### Bước 5: Đẩy mã nguồn lên GitHub
```bash
git push -u origin main
```

---

## 3. Minh chứng kết quả thực hiện

### 3.1. Kiểm tra SSH Connection
```bash
$ ssh -T git@github.com
Hi [your-username]! You've successfully authenticated, but GitHub does not provide shell access.
```

### 3.2. Kiểm tra Remote URL
```bash
$ git remote -v
origin  git@github.com:[your-username]/[your-repo].git (fetch)
origin  git@github.com:[your-username]/[your-repo].git (push)
```

### 3.3. Trạng thái sau khi Push
```bash
$ git push -u origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:[your-username]/[your-repo].git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

---

## 4. Kết luận
- Đã tạo thành công khóa xác thực SSH bằng thuật toán **Ed25519**.
- Xác thực và phân quyền giữa máy cá nhân và GitHub hoạt động chính xác mà không cần nhập token / mật khẩu mỗi lần thao tác.
- Repository đã được đồng bộ hoàn tất lên GitHub qua giao thức SSH an toàn.
