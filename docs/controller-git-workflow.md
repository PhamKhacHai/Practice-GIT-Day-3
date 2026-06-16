# Controller Git Workflow

## 1. Mục đích

Tài liệu này mô tả quy trình Git Workflow áp dụng cho dự án R&B Controller hoặc Siemens Controller. Mục tiêu của quy trình là đảm bảo mọi thay đổi đều được phát triển trên branch riêng, kiểm tra bằng CI, review chéo bởi người khác và chỉ được merge vào branch chính khi đáp ứng đủ điều kiện chất lượng.

## 2. Nguyên tắc chung

* Không commit trực tiếp vào `main` hoặc `develop`.
* Mọi thay đổi phải được thực hiện trên branch riêng.
* Mọi thay đổi phải được đưa vào `develop` thông qua Pull Request.
* Pull Request phải có mô tả rõ ràng theo template.
* Pull Request phải được review bởi ít nhất một người khác.
* CI phải chạy thành công trước khi merge.
* Người tạo Pull Request không tự review hoặc tự merge PR của mình.
* Maintainer hoặc Team Lead là người chịu trách nhiệm merge PR sau khi đủ điều kiện.

## 3. Vai trò của các branch

### `main`

Branch `main` chứa code ổn định, đã được kiểm tra và sẵn sàng cho release. Đây là branch quan trọng nhất, không được commit trực tiếp và chỉ nhận thay đổi từ `develop` hoặc hotfix đã được kiểm tra.

### `develop`

Branch `develop` là branch tích hợp chính của dự án. Các feature branch sau khi hoàn thành sẽ tạo Pull Request vào `develop`. Code trên `develop` cần luôn ở trạng thái có thể kiểm tra và chuẩn bị release.

### `feature/*`

Branch `feature/*` dùng để phát triển chức năng mới, tài liệu mới hoặc thay đổi cấu hình. Developer tạo feature branch từ `develop`, commit code, push lên GitHub và tạo Pull Request vào `develop`.

Ví dụ:

```text
feature/controller-git-workflow
feature/controller-io-monitoring
feature/controller-alarm-handling
```

### `hotfix/*`

Branch `hotfix/*` dùng để sửa lỗi khẩn cấp trên `main` hoặc lỗi nghiêm trọng cần xử lý nhanh. Sau khi hotfix được merge vào `main`, thay đổi cần được đồng bộ lại về `develop` để tránh mất code sửa lỗi.

### `release/*`

Branch `release/*` dùng để chuẩn bị phát hành phiên bản mới. Khi `develop` đã ổn định, Team Lead có thể tạo release branch để kiểm tra lần cuối, sửa lỗi nhỏ nếu có, sau đó merge vào `main`.

## 4. Quy trình phát triển

Quy trình phát triển được thực hiện theo các bước sau:

1. Developer cập nhật code mới nhất từ `develop`.
2. Developer tạo branch mới từ `develop`.
3. Developer thực hiện thay đổi, commit và push branch lên GitHub.
4. Developer tạo Pull Request vào `develop`.
5. Pull Request phải điền đầy đủ các mục trong template: Summary, Scope, Testing, Risk, Rollback Plan và Checklist.
6. GitHub Actions CI tự động chạy để kiểm tra Pull Request.
7. Reviewer kiểm tra nội dung thay đổi và comment nếu cần chỉnh sửa.
8. Developer sửa theo comment, commit và push lại lên cùng branch.
9. Reviewer approve sau khi nội dung đạt yêu cầu.
10. Maintainer hoặc Team Lead merge Pull Request vào `develop`.

## 5. Quy định Pull Request

Mỗi Pull Request cần có nội dung rõ ràng, bao gồm:

* Tóm tắt thay đổi.
* Phạm vi thay đổi.
* Cách đã kiểm tra.
* Rủi ro có thể xảy ra.
* Kế hoạch rollback nếu phát sinh lỗi.
* Checklist xác nhận không vi phạm quy trình Git.

Pull Request không được merge nếu thiếu mô tả, CI fail, chưa có approval hoặc còn unresolved comments.

## 6. Quy định review

Review phải là review thật, không chỉ approve hình thức. Reviewer cần kiểm tra:

* Nội dung thay đổi có đúng yêu cầu không.
* Có ảnh hưởng đến branch `develop` hoặc `main` không.
* Commit message có rõ ràng không.
* Tài liệu hoặc code có dễ hiểu, dễ bảo trì không.
* Có cần bổ sung test, rollback plan hoặc ghi chú rủi ro không.

Nếu phát hiện vấn đề, reviewer cần comment rõ ràng để developer sửa. Sau khi developer sửa xong, reviewer kiểm tra lại và approve nếu đạt yêu cầu.

## 7. CI/CD cơ bản

GitHub Actions CI được sử dụng để tự động kiểm tra Pull Request trước khi merge vào `develop` hoặc `main`. Workflow tối thiểu cần thực hiện:

* Checkout repository.
* In thông tin branch.
* Kiểm tra file `README.md` tồn tại.
* Chạy thêm lint hoặc test nếu dự án có source code.

CI giúp phát hiện lỗi sớm, tránh merge thay đổi chưa đạt yêu cầu vào branch chính.

## 8. Branch Protection

Branch `develop` và `main` cần được bảo vệ bằng Branch Protection hoặc Ruleset. Các rule cần có:

* Bắt buộc tạo Pull Request trước khi merge.
* Bắt buộc ít nhất 1 approval.
* Bắt buộc CI pass trước khi merge.
* Không cho force push.
* Không cho delete branch.

Việc bảo vệ branch giúp tránh commit trực tiếp, tránh ghi đè lịch sử commit và đảm bảo mọi thay đổi đều được kiểm tra trước khi tích hợp.

## 9. Rollback Plan

Nếu Pull Request sau khi merge gây lỗi, có thể rollback bằng một trong các cách sau:

* Revert Pull Request trên GitHub.
* Revert commit gây lỗi bằng `git revert`.
* Tạo hotfix branch để sửa lỗi khẩn cấp.
* Khôi phục lại phiên bản ổn định gần nhất nếu lỗi nghiêm trọng.

## 10. Kết luận

Quy trình Git Workflow cho dự án Controller giúp kiểm soát chất lượng code, giảm lỗi khi merge vào branch chính và đảm bảo mọi thay đổi đều được review độc lập. Việc kết hợp Pull Request Template, GitHub Actions CI, Branch Protection và review chéo giúp quy trình phát triển an toàn, minh bạch và phù hợp với môi trường dự án thực tế của R&B.
