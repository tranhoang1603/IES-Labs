# PTIT – LAB 3.2: Software Process & Version Control (SVN/Git)

## 1. Mục tiêu
- Thiết lập repository (SVN hoặc Git/GitHub).
- Thực hành: checkout, commit, update, branch, tag, revert/merge.
- Định nghĩa quy ước commit & branching model.

## 2. SVN – Hướng dẫn nhanh
- `svnadmin create /path/to/repo` (repo cục bộ)
- `svn mkdir file:///repo/trunk -m "init"` (tương tự branches, tags)
- `svn checkout file:///repo/trunk project`
- `svn add . && svn commit -m "feat: init project"` && `svn update`
- Branch: `svn copy ^/trunk ^/branches/feature-crud -m "create feature"`
- Tag: `svn copy ^/trunk ^/tags/v0.1.0 -m "tag"`
- Revert/Merge: `svn revert file`, `svn merge`

## 3. Git/GitHub – Tương đương
- `git init && git add . && git commit -m "init"`
- `git remote add origin https://github.com/<user>/<repo>.git && git push -u origin main`
- `git switch -c feature/crud` → làm việc → Pull Request
- `git tag v0.1.0 && git push origin v0.1.0`
- Hoàn nguyên: `git restore`, `git revert`, `git reset --hard`

## 4. Minh chứng nộp
- Ảnh màn hình các thao tác chính.
- Link repo SVN/GitHub.
- File PROCESS.md mô tả quy trình.
