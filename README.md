# **Lập kế hoạch tài chính cá nhân bằng Chain-of-Thought**

## Prompt đầu vào 
```text
Bạn là chuyên gia quản lý tài chính cá nhân, chuyên lập ngân sách cho người có thu nhập biến động và đang có nghĩa vụ trả nợ. Bạn làm việc bằng tiếng Việt, rõ ràng, thực tế và không phán xét.

## DỮ LIỆU ĐẦU VÀO
- Loại thu nhập: freelance
- Thu nhập 6 tháng gần nhất (VND): 7 triệu / tháng
- Thu nhập gộp dự kiến mỗi tháng: 7 triệu /tháng
- Có bị khấu trừ bảo hiểm và thuế tại nguồn không:  có 
- Số người phụ thuộc: 2
- Chi phí cố định hằng tháng (nhà, điện, nước, internet, đi lại, học phí, bảo hiểm tự nguyện...): 3 triệu 5 
- Chi phí biến đổi trung bình (ăn uống, mua sắm, giải trí...): 2 triệu 
- Các khoản nợ: [vay đầu tư  | 20 triệu  | lãi suất 5%/năm | số tiền trả tối thiểu là 1 tr/tháng | 11/12]
- Quỹ dự phòng hiện có: [500k]
- Mục tiêu tiết kiệm: [Mục tiêu tk được 5tr , trong vòng 2-3 tháng]
- Tỷ lệ tiết kiệm cam kết mỗi tháng: [70-80% thu nhập]

Nếu thiếu dữ liệu, hãy nêu rõ giả định bạn đưa ra và tiếp tục tính toán. Không hỏi lại quá một câu.

## RÀNG BUỘC KỸ THUẬT (BẮT BUỘC)
1. Hiển thị đầy đủ phép tính trung gian. Mọi phép cộng, trừ, nhân, chia đều phải được viết ra dạng "a × b = c" hoặc "a − b = c", không được nhảy thẳng đến kết quả. Mỗi số trung gian phải được dùng đúng ở bước sau.
2. Tổng tỷ lệ phần trăm phân bổ phải bằng chính xác 100,0%. Quy tắc làm tròn:
  - Mỗi tỷ lệ % = số tiền ÷ Thu nhập gộp × 100, làm tròn đến 0,1%.
  - Cộng tất cả các tỷ lệ. Nếu tổng khác 100,0%, cộng hoặc trừ phần chênh vào dòng có số tiền lớn nhất và ghi chú "đã điều chỉnh làm tròn".
3. Tổng số tiền các dòng phân bổ phải bằng Thu nhập gộp, sai số 0 đồng. Dòng "Chi tiêu linh hoạt" được tính bằng phép trừ còn lại (Thu nhập gộp − tổng các dòng khác), không ước lượng riêng.
4. Không viết code, không dùng Pydantic, không dùng Python hay bất kỳ ngôn ngữ lập trình nào. Mọi phép tính thực hiện bằng suy luận logic trong văn bản.
5. Làm tròn số tiền đến nghìn đồng ở mỗi bước. Ghi rõ nếu có chênh lệch do làm tròn và cách xử lý.

## QUY TRÌNH SUY LUẬN
Hãy suy nghĩ từng bước một và tính toán chi tiết trước khi đưa ra quyết định. Không bỏ qua bước nào. Ở mỗi bước, ghi rõ công thức, số liệu thay vào và kết quả trung gian.

**Bước 0: Xác định thu nhập nền (chỉ áp dụng khi thu nhập biến động)**
- Thu nhập nền = thấp hơn giữa (a) trung bình 6 tháng và (b) thu nhập của tháng thấp nhất trong 6 tháng gần nhất.
- Trung bình 6 tháng = tổng 6 tháng ÷ 6, ghi rõ phép cộng từng tháng.
- Phần thu nhập vượt nền = Thu nhập gộp − Thu nhập nền. Phần này không dùng cho chi tiêu thường xuyên mà chuyển vào Quỹ đệm thu nhập cho đến khi đạt 3 tháng chi phí cố định.
- Toàn bộ ngân sách bên dưới lập trên thu nhập nền, riêng phần vượt nền xử lý theo quy tắc trên.

**Bước 1: Tính thu nhập khả dụng sau thuế và chi phí bắt buộc**
- Bảo hiểm bắt buộc (nếu là nhân viên): 10,5% lương đóng bảo hiểm (BHXH 8%, BHYT 1,5%, BHTN 1%), tính trên mức lương đóng bảo hiểm theo quy định. Ghi rõ từng phép nhân.
- Thuế TNCN (nếu là nhân viên): Thu nhập tính thuế = Thu nhập gộp − Bảo hiểm − 11.000.000 − 4.400.000 × số người phụ thuộc. Áp dụng biểu lũy tiến theo tháng: đến 5 triệu (5%), 5–10 triệu (10%), 10–18 triệu (15%), 18–32 triệu (20%), 32–52 triệu (25%), 52–80 triệu (30%), trên 80 triệu (35%). Tính thuế theo từng bậc, cộng lại.
- Với freelance hoặc kinh doanh: ước tính thuế theo quy định áp dụng cho loại hình đó và trích một tỷ lệ dự phòng (ghi rõ giả định và phép nhân).
- Thu nhập khả dụng = Thu nhập nền − Bảo hiểm − Thuế − Tỷ lệ dự phòng thuế (nếu có).

**Bước 2: Phân bổ khoản trả nợ tối thiểu**
- Tổng nợ tối thiểu M = cộng số tiền trả tối thiểu của từng khoản vay (ghi rõ phép cộng).
- Tỷ lệ nợ trên thu nhập D = M ÷ Thu nhập nền × 100. Nếu D > 40%, cảnh báo rủi ro quá tải nợ.
- Xác định khoản lãi suất cao nhất để gợi ý ưu tiên trả thêm ở Bước 4. Không được vi phạm mức trả tối thiểu của bất kỳ khoản nào.
- Thu nhập sau nợ = Thu nhập khả dụng − M.

**Bước 3: Trích lập quỹ tiết kiệm theo tỷ lệ cam kết**
- Tiết kiệm cam kết S = Tỷ lệ cam kết × Thu nhập sau nợ.
- Kiểm tra quỹ dự phòng: Chi phí cố định hằng tháng F (tổng các khoản cộng lại). Mức an toàn = 3 × F. Nếu Quỹ dự phòng hiện có < mức an toàn, tính số tháng cần = (mức an toàn − quỹ hiện có) ÷ mức tiết kiệm khả dụng, và ưu tiên bổ sung trước các mục tiêu khác.
- Nếu Thu nhập sau nợ không đủ cho chi phí cố định, giảm tỷ lệ tiết kiệm xuống mức tối đa khả thi và ghi rõ lý do.

**Bước 4: Chia phần còn lại theo quy tắc 50/30/20 cải tiến**
- Phần còn lại R = Thu nhập sau nợ − S.
- Ba nhóm:
  - Nhu cầu thiết yếu (mục tiêu tối đa 50% thu nhập nền): chi phí cố định F + M + bảo hiểm và thuế đã tính. Tính tỷ lệ thực tế = tổng ÷ Thu nhập nền × 100.
  - Chi tiêu linh hoạt (mục tiêu 30%): phần còn lại sau khi trừ các mục bắt buộc, không vượt R.
  - Tiết kiệm và trả nợ thêm (mục tiêu 20%): gồm S, Quỹ đệm thu nhập/dự phòng và phần trả thêm ưu tiên cho khoản lãi cao nhất.
- So sánh từng nhóm với tỷ lệ mục tiêu bằng phép trừ (thực tế − mục tiêu). Nếu nhu cầu thiết yếu vượt 50%, đề xuất cắt giảm cụ thể ở chi phí cố định hoặc tái cơ cấu nợ, và tính số tiền cần cắt.

**Bước 5: Kiểm tra đối chiếu**
- Tổng các dòng phân bổ = Thu nhập gộp, sai số 0 đồng. Viết rõ phép cộng.
- Tổng tỷ lệ % = 100,0%. Viết rõ phép cộng các tỷ lệ.
- Nếu thiếu tiền, cắt theo thứ tự: chi tiêu linh hoạt trước, sau đó tiết kiệm cam kết. Không cắt mức trả nợ tối thiểu và quỹ dự phòng.
- Kiểm tra lại toàn bộ phép tính trước khi kết luận. Nếu phát hiện sai số, sửa và ghi chú.

## ĐẦU RA
Trình bày kết quả thành hai phần rõ ràng:

**Phần 1: Nhật ký suy luận (Reasoning Steps)**
Liệt kê từng bước 0–5 với công thức, phép tính đầy đủ, số liệu thay vào và kết quả trung gian.

**Phần 2: Bảng phân bổ ngân sách cuối cùng**
Dùng Markdown Table với các cột: | Hạng mục | Số tiền (VND) | Tỷ lệ % | Ghi chú |
- Tỷ lệ % tính trên Thu nhập gộp.
- Các dòng phân bổ gồm: Khấu trừ bảo hiểm, Khấu trừ thuế, Trả nợ tối thiểu, Tiết kiệm cam kết, Quỹ đệm thu nhập/dự phòng, Chi phí cố định, Chi tiêu linh hoạt, Trả nợ thêm.
- Dòng cuối là Tổng cộng, với Số tiền bằng Thu nhập gộp và Tỷ lệ % bằng 100,0%.
- Thêm dòng ghi nhớ "Thu nhập nền" (không tính vào tổng %) để người đọc thấy cơ sở lập ngân sách.

Sau bảng, viết 3 hành động ưu tiên cho tháng tới, mỗi hành động một câu cụ thể. Nếu tỷ lệ nợ trên thu nhập vượt 50% hoặc có khoản lãi suất cao như thẻ tín dụng, khuyến nghị người dùng tham khảo chuyên gia tư vấn tài chính hoặc đàm phán tái cơ cấu nợ với ngân hàng. Không đưa ra khuyến nghị đầu tư cụ thể.

QUAN TRỌNG : "KẾT QUẢ XUẤT DƯỚI DẠNG MARKDOWN"
```

## Phần 2 : Kết quả nhận về của AI


# Kế Hoạch Ngân Sách Cá Nhân & Trả Nợ (Thu Nhập Freelance)

## Giả định đầu vào
* **Bảo hiểm và thuế của freelance**: Thu nhập đến từ hợp đồng dịch vụ/vãng lai nên không bị khấu trừ BHXH, BHYT, BHTN bắt buộc tại nguồn (`0%`), nhưng bị khấu trừ thuế TNCN tại nguồn `10%` đối với khoản chi trả từ `2.000.000` VND/lần trở lên. Do tổng thu nhập năm (`84.000.000` VND) thấp hơn mức giảm trừ gia cảnh của bản thân và 2 người phụ thuộc, không cần trích thêm tỷ lệ dự phòng thuế cuối năm (`0%`).
* **Thông tin khoản nợ (`11/12`)**: Giả định `11/12` là ngày thanh toán định kỳ hằng tháng (hoặc kỳ thanh toán thứ 11/12) và giữ nguyên nghĩa vụ trả nợ tối thiểu bắt buộc là `1.000.000` VND/tháng trên dư nợ `20.000.000` VND, lãi suất `5%/năm`.
* **Tỷ lệ tiết kiệm cam kết (`70–80%`)**: Lấy mốc khởi điểm `70%` trên thu nhập sau nợ để kiểm tra tính khả thi trước khi điều chỉnh về mức thực tế.
* **Làm tròn**: Mọi số tiền đều tròn đến hàng nghìn đồng ngay từ đầu nên không phát sinh sai số làm tròn tiền tệ.

---

## Phần 1: Nhật ký suy luận (Reasoning Steps)

### Bước 0: Xác định thu nhập nền
* **Công thức**:
  * $\text{Trung bình 6 tháng} = \text{Tổng thu nhập 6 tháng} \div 6$
  * $\text{Thu nhập nền} = \min(\text{Trung bình 6 tháng}, \text{Tháng thấp nhất 6 tháng})$
  * $\text{Phần thu nhập vượt nền} = \text{Thu nhập gộp} - \text{Thu nhập nền}$
* **Tính toán**:
  * Tổng thu nhập 6 tháng gần nhất: $7.000.000 + 7.000.000 + 7.000.000 + 7.000.000 + 7.000.000 + 7.000.000 = 42.000.000\text{ VND}$
  * Trung bình 6 tháng (a): $42.000.000 \div 6 = 7.000.000\text{ VND}$
  * Tháng thấp nhất trong 6 tháng (b): $7.000.000\text{ VND}$
  * Thu nhập nền: $7.000.000\text{ VND}$ (do $a = b = 7.000.000\text{ VND}$)
  * Phần thu nhập vượt nền: $7.000.000 - 7.000.000 = 0\text{ VND}$
* **Kết luận trung gian**: Toàn bộ ngân sách được lập trên cơ sở Thu nhập nền $= 7.000.000\text{ VND}$. Phần thu nhập vượt nền bằng $0\text{ VND}$ nên chưa có nguồn tự động bổ sung vào Quỹ đệm thu nhập.

### Bước 1: Tính thu nhập khả dụng sau thuế và chi phí bắt buộc
* **Công thức**:
  * $\text{Khấu trừ bảo hiểm} = \text{Thu nhập nền} \times 0\%$
  * $\text{Khấu trừ thuế TNCN} = \text{Thu nhập nền} \times 10\%$
  * $\text{Dự phòng thuế bổ sung} = \text{Thu nhập nền} \times 0\%$
  * $\text{Thu nhập khả dụng} = \text{Thu nhập nền} - \text{Bảo hiểm} - \text{Thuế} - \text{Dự phòng thuế}$
* **Tính toán**:
  * Khấu trừ bảo hiểm bắt buộc: $7.000.000 \times 0\% = 0\text{ VND}$
  * Khấu trừ thuế TNCN tại nguồn: $7.000.000 \times 10\% = 700.000\text{ VND}$
  * Kiểm tra ngưỡng giảm trừ gia cảnh năm để xác định dự phòng thuế:
    * Thu nhập gộp 12 tháng: $7.000.000 \times 12 = 84.000.000\text{ VND}$
    * Giảm trừ 2 người phụ thuộc/tháng: $4.400.000 \times 2 = 8.800.000\text{ VND}$
    * Tổng giảm trừ gia cảnh/tháng: $11.000.000 + 8.800.000 = 19.800.000\text{ VND}$
    * Tổng giảm trừ gia cảnh 12 tháng: $19.800.000 \times 12 = 237.600.000\text{ VND}$
    * Vì $84.000.000 < 237.600.000$, dự phòng thuế bổ sung: $7.000.000 \times 0\% = 0\text{ VND}$
  * Thu nhập khả dụng: $7.000.000 - 0 - 700.000 - 0 = 6.300.000\text{ VND}$

### Bước 2: Phân bổ khoản trả nợ tối thiểu
* **Công thức**:
  * $\text{Tổng nợ tối thiểu } M = \text{Khoản trả tối thiểu khoản vay đầu tư}$
  * $\text{Tỷ lệ nợ trên thu nhập } D = M \div \text{Thu nhập nền} \times 100$
  * $\text{Thu nhập sau nợ} = \text{Thu nhập khả dụng} - M$
* **Tính toán**:
  * Tổng nợ tối thiểu $M$: $1.000.000\text{ VND}$
  * Tỷ lệ nợ trên thu nhập $D$: $1.000.000 \div 7.000.000 \times 100 = 14,2857\%$, làm tròn thành $14,3\%$
  * Đánh giá rủi ro: $14,3\% < 40\%$, nghĩa vụ trả nợ tối thiểu nằm trong vùng an toàn. Khoản nợ duy nhất có lãi suất $5\%/\text{năm}$ (lãi suất thấp), nên chưa cần ưu tiên trả trước hạn khi quỹ dự phòng còn mỏng.
  * Thu nhập sau nợ: $6.300.000 - 1.000.000 = 5.300.000\text{ VND}$

### Bước 3: Trích lập quỹ tiết kiệm theo tỷ lệ cam kết và kiểm tra quỹ dự phòng
* **Kiểm tra khả năng trích tiết kiệm theo cam kết 70%**:
  * Tiết kiệm cam kết mong muốn (mức $70\%$): $5.300.000 \times 70\% = 3.710.000\text{ VND}$
  * Số tiền còn lại sau khi trích $3.710.000\text{ VND}$: $5.300.000 - 3.710.000 = 1.590.000\text{ VND}$
  * So sánh với Chi phí cố định hằng tháng $F = 3.500.000\text{ VND}$: Số tiền còn lại $1.590.000\text{ VND}$ không đủ trả chi phí cố định, bị thiếu hụt $3.500.000 - 1.590.000 = 1.910.000\text{ VND}$ (chưa tính chi phí ăn uống, sinh hoạt cho 3 người).
  * **Điều chỉnh tỷ lệ tiết kiệm về mức tối đa khả thi**:
    * Phần dư tối đa sau khi trừ chi phí cố định: $5.300.000 - 3.500.000 = 1.800.000\text{ VND}$ (tương đương $1.800.000 \div 5.300.000 \times 100 = 33,9623\% \approx 34,0\%$ thu nhập sau nợ).
    * Tuy nhiên, để duy trì tối thiểu $800.000\text{ VND/tháng}$ cho chi tiêu ăn uống sinh hoạt thiết yếu (cắt giảm $2.000.000 - 800.000 = 1.200.000\text{ VND}$ so với mức chi biến đổi trung bình hiện tại), mức tiết kiệm khả thi thực tế $S$ là:
      $$S = 1.800.000 - 800.000 = 1.000.000\text{ VND}$$
    * Tỷ lệ tiết kiệm điều chỉnh trên thu nhập sau nợ: $1.000.000 \div 5.300.000 \times 100 = 18,8679\%$, làm tròn thành $18,9\%$.
* **Kiểm tra Quỹ dự phòng**:
  * Chi phí cố định hằng tháng $F$: $3.500.000\text{ VND}$
  * Mức an toàn quỹ dự phòng: $3 \times 3.500.000 = 10.500.000\text{ VND}$
  * Quỹ dự phòng hiện có: $500.000\text{ VND}$
  * Phần quỹ dự phòng còn thiếu: $10.500.000 - 500.000 = 10.000.000\text{ VND}$
  * Số tháng cần để đạt mức an toàn với mức tiết kiệm khả dụng $1.000.000\text{ VND/tháng}$:
    $$10.000.000 \div 1.000.000 = 10\text{ tháng}$$
  * **Ưu tiên phân bổ**: Vì $500.000 < 10.500.000$, toàn bộ $1.000.000\text{ VND}$ tiết kiệm khả dụng hằng tháng được ưu tiên đưa vào **Quỹ đệm thu nhập/dự phòng** trước khi thực hiện mục tiêu tiết kiệm riêng $5.000.000\text{ VND}$ (do đó dòng Tiết kiệm cam kết cho mục tiêu riêng tạm thời là $0\text{ VND}$).

### Bước 4: Chia phần còn lại theo quy tắc 50/30/20 cải tiến
* **Tính phần còn lại $R$**:
  * $R = \text{Thu nhập sau nợ} - S = 5.300.000 - 1.000.000 = 4.300.000\text{ VND}$
* **Phân tích 3 nhóm trên nền thu nhập $7.000.000\text{ VND}$**:
  1. **Nhóm Nhu cầu thiết yếu (mục tiêu tối đa $50,0\%$)**:
     * Tổng chi thiết yếu $= F + M + \text{Bảo hiểm} + \text{Thuế} = 3.500.000 + 1.000.000 + 0 + 700.000 = 5.200.000\text{ VND}$
     * Tỷ lệ thực tế: $5.200.000 \div 7.000.000 \times 100 = 74,2857\%$, làm tròn thành $74,3\%$
     * So sánh với mục tiêu: $74,3\% - 50,0\% = +24,3\%$ (vượt trần $24,3\%$)
     * Tính số tiền vượt trần cần cắt giảm hoặc bù đắp:
       * Trần $50\%$ thu nhập nền: $7.000.000 \times 50\% = 3.500.000\text{ VND}$
       * Số tiền vượt trần: $5.200.000 - 3.500.000 = 1.700.000\text{ VND/tháng}$
     * *Đề xuất xử lý*: Cần cắt giảm ít nhất $1.700.000\text{ VND/tháng}$ ở chi phí cố định (như chuyển sang chỗ ở chi phí thấp hơn, tối ưu điện nước/viễn thông, rà soát tạm dừng bảo hiểm tự nguyện chưa cấp bách) hoặc đàm phán giãn kỳ hạn khoản vay $20.000.000\text{ VND}$ để hạ mức trả tối thiểu xuống khoảng $500.000\text{ VND/tháng}$, kết hợp tăng thu nhập freelance thêm tối thiểu $1.700.000\text{ VND/tháng}$.
  2. **Nhóm Chi tiêu linh hoạt (mục tiêu $30,0\%$)**:
     * Phần còn lại sau khi trừ chi phí cố định từ $R$: $4.300.000 - 3.500.000 = 800.000\text{ VND}$ (thỏa mãn điều kiện $\le R$)
     * Tỷ lệ thực tế: $800.000 \div 7.000.000 \times 100 = 11,4286\%$, làm tròn thành $11,4\%$
     * So sánh với mục tiêu: $11,4\% - 30,0\% = -18,6\%$ (thấp hơn mục tiêu $18,6\%$)
  3. **Nhóm Tiết kiệm và trả nợ thêm (mục tiêu $20,0\%$)**:
     * Tổng nhóm $= \text{Tiết kiệm cam kết riêng} + \text{Quỹ đệm thu nhập/dự phòng} + \text{Trả nợ thêm} = 0 + 1.000.000 + 0 = 1.000.000\text{ VND}$
     * Tỷ lệ thực tế: $1.000.000 \div 7.000.000 \times 100 = 14,2857\%$, làm tròn thành $14,3\%$
     * So sánh với mục tiêu: $14,3\% - 20,0\% = -5,7\%$ (thấp hơn mục tiêu $5,7\%$)

### Bước 5: Kiểm tra đối chiếu
* **Tính dòng Chi tiêu linh hoạt bằng phép trừ phần còn lại**:
  * Tổng các dòng phân bổ khác: $0 + 700.000 + 1.000.000 + 0 + 1.000.000 + 3.500.000 + 0 = 6.200.000\text{ VND}$
  * Chi tiêu linh hoạt: $7.000.000 - 6.200.000 = 800.000\text{ VND}$
* **Kiểm tra tổng số tiền phân bổ**:
  * $0 + 700.000 + 1.000.000 + 0 + 1.000.000 + 3.500.000 + 800.000 + 0 = 7.000.000\text{ VND}$ (khớp chính xác Thu nhập gộp, sai số $0\text{ VND}$).
* **Tính và kiểm tra tổng tỷ lệ % trên Thu nhập gộp ($7.000.000\text{ VND}$)**:
  * Khấu trừ bảo hiểm: $0 \div 7.000.000 \times 100 = 0,0\%$
  * Khấu trừ thuế: $700.000 \div 7.000.000 \times 100 = 10,0\%$
  * Trả nợ tối thiểu: $1.000.000 \div 7.000.000 \times 100 = 14,2857\% \approx 14,3\%$
  * Tiết kiệm cam kết: $0 \div 7.000.000 \times 100 = 0,0\%$
  * Quỹ đệm thu nhập/dự phòng: $1.000.000 \div 7.000.000 \times 100 = 14,2857\% \approx 14,3\%$
  * Chi phí cố định: $3.500.000 \div 7.000.000 \times 100 = 50,0\%$
  * Chi tiêu linh hoạt: $800.000 \div 7.000.000 \times 100 = 11,4286\% \approx 11,4\%$
  * Trả nợ thêm: $0 \div 7.000.000 \times 100 = 0,0\%$
  * Tổng cộng tỷ lệ %: $0,0\% + 10,0\% + 14,3\% + 0,0\% + 14,3\% + 50,0\% + 11,4\% + 0,0\% = 100,0\%$ (khớp chính xác $100,0\%$, không cần điều chỉnh làm tròn).

---

## Phần 2: Bảng phân bổ ngân sách cuối cùng

| Hạng mục | Số tiền (VND) | Tỷ lệ % | Ghi chú |
| :--- | :---: | :---: | :--- |
| Khấu trừ bảo hiểm | 0 | 0,0% | Freelance không thuộc diện đóng BHXH/BHYT/BHTN bắt buộc tại nguồn |
| Khấu trừ thuế | 700.000 | 10,0% | Khấu trừ thuế TNCN tại nguồn 10% cho thu nhập vãng lai/dịch vụ |
| Trả nợ tối thiểu | 1.000.000 | 14,3% | Khoản vay đầu tư 20 triệu (lãi suất 5%/năm), tỷ lệ nợ an toàn |
| Tiết kiệm cam kết | 0 | 0,0% | Tạm hoãn mục tiêu riêng 5 triệu để ưu tiên xây quỹ dự phòng trước |
| Quỹ đệm thu nhập/dự phòng | 1.000.000 | 14,3% | Dồn toàn bộ năng lực tiết kiệm khả thi để đạt mốc an toàn 10,5 triệu |
| Chi phí cố định | 3.500.000 | 50,0% | Nhà ở, điện, nước, internet, đi lại, học phí, bảo hiểm tự nguyện |
| Chi tiêu linh hoạt | 800.000 | 11,4% | Phần còn lại cho ăn uống, sinh hoạt tối thiểu (cắt giảm 1,2 triệu) |
| Trả nợ thêm | 0 | 0,0% | Tạm hoãn vì lãi suất vay 5%/năm thấp, ưu tiên hoàn thiện quỹ dự phòng |
| **Tổng cộng** | **7.000.000** | **100,0%** | **Khớp 100% Thu nhập gộp dự kiến (sai số 0 đồng)** |
| *Thu nhập nền (Ghi nhớ)* | *7.000.000* | *-* | *Cơ sở lập ngân sách (không tính vào tổng %); phần vượt nền = 0 VND* |

### 3 hành động ưu tiên cho tháng tới
1. **Rà soát và cắt giảm ngay ít nhất 1.000.000 – 1.700.000 VND trong nhóm chi phí cố định 3.500.000 VND** bằng cách tối ưu tiền thuê nhà, hóa đơn tiện ích hoặc tạm dừng các khoản bảo hiểm tự nguyện chưa cấp bách để giảm áp lực chi tiêu sinh hoạt cho gia đình.
2. **Thiết lập chuyển khoản tự động 1.000.000 VND vào tài khoản Quỹ dự phòng riêng biệt ngay khi nhận thanh toán freelance** (sau khi trừ thuế 700.000 VND và nợ 1.000.000 VND) để từng bước nâng quỹ từ 500.000 VND lên mức an toàn 10.500.000 VND trong 10 tháng tới.
3. **Tìm kiếm thêm dự án freelance ngắn hạn hoặc đăng ký thủ tục hoàn thuế/cam kết thu nhập cuối năm (mẫu 08/CK-TNCN)** nếu đủ điều kiện theo quy định thuế hiện hành nhằm tăng dòng tiền ròng thực nhận ít nhất 1.500.000 – 2.000.000 VND/tháng để sớm khởi động lại mục tiêu tiết kiệm 5.000.000 VND.

