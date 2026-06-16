# Day 3 Review Practice

## Purpose

Tài liệu này dùng để thực hành quy trình Pull Request, review chéo, chỉnh sửa theo comment và merge theo Branch Protection.

## Git Workflow

Developer tạo branch riêng từ develop, commit code, push lên GitHub và tạo Pull Request vào develop.

Reviewer kiểm tra nội dung thay đổi, để lại comment nếu phát hiện vấn đề hoặc cần cải thiện.

Developer sửa theo comment, commit lại và push lên cùng branch.

Sau khi reviewer approve và CI pass, Maintainer hoặc Team Lead sẽ merge Pull Request vào develop.

## Notes

- Không commit trực tiếp vào main hoặc develop.
- Không tự merge Pull Request của mình.
- Pull Request cần có mô tả rõ ràng.
- CI cần pass trước khi merge.
- Cần ít nhất một approval trước khi merge.