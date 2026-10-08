# Báo Cáo Đánh Giá RAGAS: Prompt V1 vs Prompt V2

**Học viên:** Ngô Kỳ Anh  
**MSSV:** 2A202602916  
**Dự án:** Day 22 - LLMOps Prompt Versioning & Evaluation  

---

## 1. Bảng Kết Quả So Sánh Thực Tế

| Chỉ số RAGAS | Prompt V1 (Ngắn gọn) | Prompt V2 (Chuyên gia/Cấu trúc) | Nhận xét / Winner |
|---|:---:|:---:|:---:|
| **Faithfulness** | **0.9742** | **0.9458** | ← V1 thắng (Cả hai đều ≥ 0.9 đạt điểm thưởng) |
| **Answer Relevancy** | **0.9181** | 0.8871 | ← V1 thắng (0.9181 > 0.8871) |
| **Context Recall** | **1.0000** | **1.0000** | Ngang nhau (100% ground truth được truy xuất) |
| **Context Precision** | **0.9537** | 0.9511 | ← V1 thắng |

---

## 2. Phân Tích & Giải Thích Chi Tiết

### Tại sao Prompt V1 đạt điểm Faithfulness & Relevancy cao hơn V2?

1. **Về Faithfulness (Độ trung thực với tài liệu):**
   - **Prompt V1 (`SYSTEM_V1`):** Yêu cầu *"trả lời ngắn gọn (2-4 câu), chỉ dựa trên context. Nếu không có thông tin, hãy nói thẳng là không biết"*. Phong cách trả lời súc tích, ngắn gọn giúp mô hình bám sát trực tiếp vào các câu văn nguyên bản trong context được truy xuất, giảm thiểu tối đa hiện tượng diễn giải lan man hay suy diễn ngoài lề. Nhờ vậy, điểm **Faithfulness của V1 đạt mức xuất sắc: 0.9742**.
   - **Prompt V2 (`SYSTEM_V2`):** Yêu cầu vai trò chuyên gia, phân tích và tổ chức câu trả lời 3-5 câu. Dù nội dung giải thích đầy đủ và logic, việc mở rộng cấu trúc phân tích khiến câu trả lời dài hơn và đôi khi dùng thêm các từ ngữ liên kết tổng hợp, làm điểm faithfulness thấp hơn một chút (**0.9458**), dù vẫn vượt xa mốc điểm thưởng 0.9.

2. **Về Answer Relevancy (Độ liên quan câu trả lời):**
   - Do câu trả lời của Prompt V1 ngắn gọn, đi thẳng vào trọng tâm câu hỏi mà không có thông tin phụ trợ, các embedding của câu trả lời khớp rất cao với câu hỏi gốc, mang lại **Answer Relevancy đạt 0.9181**.
   - Câu trả lời của Prompt V2 chi tiết hơn nên độ cô đọng giảm nhẹ, đạt **0.8871**.

3. **Về Context Recall & Context Precision:**
   - Cả hai phiên bản đều dùng chung FAISS vector store với $k=3$, đạt **Context Recall = 1.0000** (toàn bộ facts trong đáp án chuẩn đều nằm trong các chunk được truy xuất).
   - Context Precision ở mức **~0.95** cho cả hai phiên bản chứng minh hệ thống retriever xếp hạng các đoạn tài liệu phù hợp nhất lên hàng đầu cực kỳ chính xác.

---

## 3. Kết Luận
Cả 2 phiên bản prompt đều đạt chuẩn xuất sắc với **Faithfulness ≥ 0.9** (vượt xa tiêu chuẩn tối thiểu 0.8 của bài lab), đủ điều kiện nhận điểm thưởng tối đa. 
- **Prompt V1** là lựa chọn lý tưởng cho các ứng dụng hỏi đáp yêu cầu phản hồi nhanh, trực diện, không lan man (như chatbot hỏi đáp chính sách, FAQ).
- **Prompt V2** phù hợp với các ứng dụng tư vấn chuyên sâu, phân tích kỹ thuật nơi người dùng cần câu trả lời có cấu trúc và giải thích mạch lạc.
