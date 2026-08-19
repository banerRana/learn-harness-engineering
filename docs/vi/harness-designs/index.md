# Phân tích các harness tiên tiến

Chuyên mục này đối chiếu lý thuyết về harness trong giáo trình với từng sản phẩm thực tế tiên tiến nhất hiện nay. Với mỗi sản phẩm, chúng ta chỉ quan tâm một điều: **harness của nó được thiết kế như thế nào** — tức lớp hạ tầng kỹ thuật bao quanh mô hình, gồm năm hệ thống con là chỉ dẫn, công cụ, môi trường, trạng thái và phản hồi, cùng các cơ chế cốt lõi như tính liên tục của ngữ cảnh, khởi tạo, xác minh, khả năng quan sát, bàn giao và vòng lặp.

Chúng ta chủ ý không bàn về năng lực suy luận của mô hình mạnh đến đâu, điểm benchmark trong một lần chạy cao hay thấp, cũng không giới thiệu chung chung rằng "agent này làm được gì". Đó là những vấn đề thuộc tầng mô hình và tầng sản phẩm. Ở đây chỉ phân tích harness — mọi thứ nằm ngoài trọng số mô hình.

## Vì sao đáng phân tích

Bài đầu tiên của giáo trình đã chỉ ra: mô hình mạnh không đồng nghĩa với thực thi đáng tin cậy. Cùng một mô hình nhưng được đặt trong các harness khác nhau có thể cho hiệu quả chênh lệch cả một bậc độ lớn. Tuy nhiên, giáo trình nói về "nên làm như thế nào", còn các sản phẩm này trả lời câu hỏi "những đội ngũ hàng đầu thực tế làm như thế nào".

Mỗi sản phẩm là một tập hợp quyết định thiết kế độc lập. Khi đặt chúng cạnh nhau để đối chiếu, bạn sẽ thấy cùng những cơ chế cốt lõi lại được các đội ngũ triển khai theo những cách hoàn toàn khác nhau:

- **Pi** xây dựng harness theo mô hình lõi tối giản + phần mở rộng có thể lập trình, dùng "system prompt tối thiểu + tải theo nhu cầu" để thực hiện kỹ thuật ngữ cảnh.
- **Claude Code** biến harness thành một môi trường chạy hoàn chỉnh: bộ nhớ phân tầng, nén năm cấp, quyền hạn, hook và agent con.
- **Codex** đẩy triết lý harness đến cực hạn: repository là nguồn sự thật, AGENTS.md chỉ là trang mục lục, còn môi trường được cô lập bằng worktree.
- **DeepSeek Harness** định nghĩa thẳng harness là một runtime độc lập với mô hình: Everything is a Plugin.

## Danh sách bài viết

- [Phân tích thiết kế harness của Pi](./pi/): lõi tối giản + phần mở rộng có thể lập trình, đưa kỹ thuật ngữ cảnh ra ngoài system prompt.
- [Phân tích thiết kế harness của Claude Code](./claude-code/): bộ nhớ phân tầng, nén năm cấp, quyền hạn và hook — một môi trường chạy agent hoàn chỉnh.
- [Phân tích thiết kế harness của Codex](./codex/): repository là nguồn sự thật, AGENTS.md là trang mục lục, cùng cơ chế cô lập môi trường và vòng phản hồi.
- [Phân tích thiết kế DeepSeek Harness](./deepseek/): Everything is a Plugin, biến chính vòng lặp agent thành một plugin có thể thay thế.

## Cách đọc

Bạn nên đọc trước vài bài đầu của giáo trình (đặc biệt là [Bài 2: Harness thực chất là gì](../lectures/lecture-02-what-a-harness-actually-is/)) để nắm khung năm hệ thống con, sau đó quay lại đây xem các sản phẩm thực tế triển khai những cơ chế này như thế nào.

Cuối mỗi bài đều có hai phần "Ánh xạ vào khung của khóa học" và "Những thiết kế đáng học hỏi", giúp bạn nhanh chóng chuyển thiết kế sản phẩm trở lại các khái niệm trong khóa học để có thể áp dụng trực tiếp vào dự án của mình.
