# Phân tích yêu cầu Lab01 – Tác tử thông minh

> Tổng hợp từ file đề bài `TTNTNC - Lab01 - Tac tu thong minh.pdf` và rà soát toàn bộ source code hiện có trong thư mục dự án.

## 0. Cơ sở lý thuyết: Tác tử thông minh (Intelligent Agents)

### 0.1 Tác tử và môi trường

- **Agent (tác tử):** bất kỳ thực thể nào có thể *cảm nhận* (perceive) môi trường của nó qua **sensors** và *hành động* (act) lên môi trường qua **actuators**. Ví dụ: con người (mắt/tai/... là sensor, tay/chân/... là actuator), robot (camera/sonar vs động cơ/bánh xe), phần mềm (bàn phím/file đầu vào vs màn hình/mạng).
- **Percept:** đầu vào cảm nhận của tác tử tại một thời điểm cụ thể.
- **Percept sequence:** toàn bộ lịch sử mọi thứ tác tử từng cảm nhận được.
- **Agent function:** ánh xạ trừu tượng từ *percept sequence* → *action*. Về lý thuyết có thể biểu diễn như một bảng (vô hạn) liệt kê mọi chuỗi percept có thể và hành động tương ứng.
- **Agent program:** cài đặt cụ thể (chương trình chạy được) hiện thực hoá agent function trên một kiến trúc (architecture) vật lý hoặc phần mềm.

### 0.2 Tác tử hợp lý (Rational Agent)

**Định nghĩa:** Với mỗi percept sequence, một tác tử hợp lý phải chọn hành động được kỳ vọng sẽ tối đa hoá **performance measure**, dựa trên bằng chứng do percept sequence cung cấp và tri thức sẵn có (prior knowledge) mà tác tử có.

Bốn yếu tố quyết định tính hợp lý tại một thời điểm:
1. Performance measure định nghĩa tiêu chí thành công.
2. Tri thức sẵn có (prior knowledge) về môi trường.
3. Tập hành động (actions) mà tác tử có thể thực hiện.
4. Chuỗi percept mà tác tử đã nhận được tính đến thời điểm đó.

Lưu ý: hợp lý (rational) **không** đồng nghĩa với toàn tri (omniscient), tiên tri (clairvoyant), hay luôn luôn thành công — chỉ là ra quyết định tốt nhất *dựa trên thông tin đang có*. **Autonomy** (tính tự chủ) là mức độ tác tử dựa vào tri thức có sẵn thay vì dựa vào percept trực tiếp từ môi trường.

### 0.3 PEAS — khung phân tích thiết kế tác tử

| Thành phần | Ý nghĩa |
|---|---|
| **P**erformance Measure | Tiêu chí đo lường mức độ thành công của tác tử |
| **E**nvironment | Không gian/thế giới mà tác tử hoạt động trong đó |
| **A**ctuators | Các hành động tác tử có thể thực hiện lên môi trường |
| **S**ensors | Thông tin tác tử có thể quan sát/cảm nhận từ môi trường |

Đây luôn là bước đầu tiên bắt buộc khi thiết kế một tác tử thông minh (Bài 1 của lab).

### 0.4 Thuộc tính của môi trường (task environment properties)

| Thuộc tính | Giải thích |
|---|---|
| Fully vs Partially observable | Sensor có cung cấp đầy đủ trạng thái liên quan để ra quyết định hay không |
| Single-agent vs Multi-agent | Một tác tử hay nhiều tác tử cùng hoạt động (cạnh tranh/hợp tác) |
| Deterministic vs Stochastic | Trạng thái kế tiếp được xác định hoàn toàn bởi trạng thái hiện tại + hành động, hay có yếu tố ngẫu nhiên |
| Episodic vs Sequential | Mỗi "episode" (cảm nhận–hành động) độc lập, hay hành động hiện tại ảnh hưởng đến các quyết định tương lai |
| Static vs Dynamic | Môi trường có thay đổi trong lúc tác tử đang "suy nghĩ" hay không (semi-dynamic: môi trường không đổi nhưng performance measure trừ điểm theo thời gian) |
| Discrete vs Continuous | Trạng thái/thời gian/percept/action rời rạc hay liên tục |
| Known vs Unknown | Tác tử (hoặc người thiết kế) có biết trước "luật chơi"/mô hình môi trường hay không — khác với observable |

### 0.5 Các loại kiến trúc tác tử (agent program types)

1. **Simple reflex agent:** chọn hành động chỉ dựa trên **percept hiện tại**, dùng tập luật điều kiện–hành động (condition–action rule, "if-then"). Không có bộ nhớ. Hoạt động tốt khi môi trường **fully observable**; dễ bị kẹt (vô hạn vòng lặp) khi môi trường **partially observable**.
2. **Model-based reflex agent:** duy trì một **trạng thái nội bộ** (internal state) được cập nhật theo percept history, dựa trên hai loại tri thức: *transition model* (thế giới tiến triển ra sao độc lập với tác tử) và *sensor model* (hành động của tác tử ảnh hưởng thế giới ra sao / percept phản ánh thế giới ra sao). Nhờ đó có thể suy luận về phần trạng thái hiện tại **không quan sát trực tiếp được**.
3. **Goal-based agent:** có một **mục tiêu** (goal) rõ ràng; cần suy luận/tìm kiếm (search, planning) để chọn chuỗi hành động dẫn tới goal. Linh hoạt hơn reflex agent vì có thể thay đổi goal mà không cần viết lại toàn bộ luật.
4. **Utility-based agent:** khi goal không đủ phân biệt "tốt" và "tốt hơn" giữa nhiều trạng thái đạt goal, cần một **hàm hữu dụng (utility function)** ánh xạ trạng thái → số thực đo "độ mong muốn". Cho phép đánh đổi (trade-off) giữa các mục tiêu xung đột, và khi kết hợp với xác suất, tác tử chọn hành động **tối đa hoá utility kỳ vọng** (expected utility) trong môi trường bất định.
5. **Learning agent:** gồm 4 thành phần — *performance element* (chọn hành động, tương đương agent ở trên), *learning element* (cải thiện performance element theo thời gian), *critic* (đánh giá hành động dựa trên performance standard cố định từ bên ngoài), *problem generator* (đề xuất hành động khám phá để học thêm).

### 0.6 Vận dụng lý thuyết vào 3 bài toán của Lab01

#### a) Robot hút bụi (Bài 2)

| PEAS | Nội dung |
|---|---|
| Performance | Tổng năng lượng (số hành động) dùng để làm sạch toàn bộ phòng — càng ít càng tốt |
| Environment | Phòng lưới n×n, mỗi ô sạch/bẩn (p=0.2 ban đầu bẩn), vị trí robot xuất phát ngẫu nhiên |
| Actuators | `suck`, `north`, `east`, `south`, `west` |
| Sensors | 4 cảm biến va chạm (N/E/S/W) + 1 cảm biến bụi (dirty) tại ô hiện tại |

- **Môi trường:** *partially observable* ở cấp toàn cục (percept chỉ cho biết trạng thái ô hiện tại + 4 hướng lân cận, không cho biết toàn bộ bản đồ bẩn/sạch), deterministic, sequential, static, discrete, single-agent.
- **randomized agent** không hợp lý (not rational) vì bỏ qua toàn bộ sensor.
- **simple reflex agent** phản xạ theo luật cục bộ (dirty→suck; sắp đâm tường→đổi hướng) nhưng **không đảm bảo dọn hết phòng** vì không nhớ đã đi đâu (dễ đi lặp lại, bỏ sót ô).
- **model-based reflex agent** cần thêm trạng thái nội bộ (bản đồ đã đi qua / vị trí hiện tại được suy luận từ lịch sử hành động) để duyệt phòng có hệ thống (ví dụ chiến lược boustrophedon giống DFS) — đây là lý do bài yêu cầu agent này "model-based": phải **suy luận trạng thái không quan sát trực tiếp** (map tổng thể) từ percept cục bộ + lịch sử hành động.

#### b) GridHunt (Bài 3, 4)

| PEAS | Nội dung |
|---|---|
| Performance | Số bước hunter cần để bắt được monster (càng ít càng tốt); `nan` nếu không bắt được trong giới hạn bước |
| Environment | Đấu trường n×n, hunter & monster xuất phát ngẫu nhiên, không được ra ngoài biên |
| Actuators | `north`, `east`, `west`, `south`, `teleport`, (Bài 3 có thêm `listen`) |
| Sensors | Vị trí bản thân luôn biết; vị trí monster — Bài 3: chỉ biết sau khi `listen`; Bài 4: biết liên tục |

- **Bài 3 (đơn tác tử):** *partially observable* (vị trí monster chỉ biết tức thời sau `listen`, sau đó "cũ dần" vì monster tiếp tục di chuyển), sequential, **dynamic** (monster di chuyển song song, không chờ hunter), discrete, stochastic (monster hành động ngẫu nhiên có trọng số), single-agent.
  → Cần **model-based reflex agent**: nhớ "vị trí monster nghe được lần cuối" làm ước lượng trạng thái hiện tại, sau đó áp dụng luật di chuyển giảm khoảng cách (tương tự thuật toán tham lam/greedy trên khoảng cách Manhattan).
- **Bài 4 (đa tác tử):** giống Bài 3 nhưng *fully observable* về vị trí monster (không cần `listen`), và là **multi-agent competitive environment** (2 hunter cạnh tranh bắt cùng 1 monster) — đây là điểm khác biệt lý thuyết quan trọng: chiến lược tối ưu của một hunter phụ thuộc cả vào monster lẫn hunter còn lại (có thể cần chiến lược "chặn đường" thay vì chỉ đuổi theo ngắn nhất).
- **teleport** hữu ích khi hunter ở quá xa vị trí monster đã biết (giảm số bước di chuyển tuần tự) nhưng có hại khi hunter đang ở gần/đang bám sát monster (teleport sẽ "reset" lợi thế vị trí một cách ngẫu nhiên).

#### c) Lunar Lander (Bài 5)

| PEAS | Nội dung |
|---|---|
| Performance | Phần thưởng: +100 khi hạ cánh an toàn, −100 khi va chạm/rơi |
| Environment | Không gian liên tục, tọa độ x,y ∈ [-2.5, 2.5], bãi đáp tại (0,0) |
| Actuators | 4 hành động rời rạc: 0-không làm gì, 1-động cơ trái, 2-động cơ chính, 3-động cơ phải |
| Sensors | Vector quan sát 8 chiều: x, y, vận tốc x, vận tốc y, góc nghiêng, vận tốc góc, 2 cảm biến tiếp đất (chân trái/phải) |

- **Môi trường:** *fully observable* (vector 8 chiều mô tả đầy đủ trạng thái động lực học cần thiết), *continuous* (không gian trạng thái/thời gian liên tục dù hành động rời rạc — theo AI: "vẫn có thể coi discrete-action nhưng continuous-state"), **deterministic** (theo tài liệu Gymnasium: `Nondeterministic: False`), sequential, **dynamic** (vật lý mô phỏng tiếp diễn theo thời gian dù agent không hành động — tàu tiếp tục rơi), single-agent.
- **simple reflex agent** khả thi về mặt lý thuyết vì môi trường fully observable (không cần nhớ trạng thái), nhưng vì không gian trạng thái là *continuous* nên các luật if/threshold rời rạc (ví dụ chỉ dựa vận tốc rơi) khó bao phủ hết mọi tình huống → cần kết hợp nhiều luật (điều khiển động cơ trái/phải theo vị trí ngang, vận tốc ngang, góc nghiêng) và **hiệu năng vẫn có giới hạn tự nhiên** vì reflex agent không lập kế hoạch nhiều bước (multi-step planning/optimal control) — đây chính là nội dung PDF yêu cầu giải thích ở Bài 5.

### 0.7 Bảng tổng hợp Performance Measure & tính hợp lý theo từng bài

| Bài toán | Performance measure | "Hợp lý" (rational) nghĩa là |
|---|---|---|
| Robot vacuum | Tổng số hành động (energy) để dọn sạch toàn phòng | Tối thiểu hoá số bước trong khi vẫn dọn sạch 100% ô bẩn |
| GridHunt | Số bước để bắt được monster (nan nếu thất bại) | Tối đa hoá tỉ lệ bắt được & tối thiểu hoá số bước trung bình |
| Lunar Lander | Reward (+100 hạ cánh an toàn / −100 va chạm), không dùng reward trung gian | Tối đa hoá reward kỳ vọng (tỉ lệ hạ cánh an toàn) qua nhiều episode |

## 1. Cấu trúc thư mục hiện tại

| File | Vai trò |
|---|---|
| `robot_vacuum.ipynb` | Bài 2 – Robot hút bụi |
| `gridhunt_mini_assignment.ipynb` | Bài 3 – GridHunt đơn tác tử |
| `gridhunt_multiagent_mini_assignment.ipynb` | Bài 4 – GridHunt đa tác tử |
| `lunar_lander.ipynb` | Bài 5 – Lunar Lander |
| `gymnasium_display_recorder.py` | Helper ghi video cho Lunar Lander (đã hoàn chỉnh, không cần sửa) |
| `gridhunt.png`, `Gridhunt2_The famous Gridhunt game.html` | Tài nguyên minh họa cho GridHunt |
| `videos/*.mp4` | Video mẫu (từ random agent) — sẽ cần chạy lại sau khi cài agent thật |
| `README.md` | Link gốc tới bản Colab của tác giả (mhahsler) — chỉ mang tính tham khảo |

⚠️ **Không có `baocao.docx`** trong thư mục — cần tạo mới theo yêu cầu nộp bài.

⚠️ **Sai khác tên file so với PDF:** PDF (mục 2, Bài 2 và Bài 3) ghi `robot_vacuum_VI.ipynb` và `gridhunt_mini_assignment_VI.ipynb`, nhưng file thực tế không có hậu tố `_VI`. Đây có thể là bản dịch tiếng Việt dự kiến nhưng giảng viên chưa cung cấp — nên làm việc trực tiếp trên file gốc hiện có, hoặc hỏi giảng viên xác nhận.

## 2. Đối chiếu Bài 1–5 (PDF) với trạng thái code

### Bài 1 – PEAS (không có notebook, làm trong báo cáo)
Không có code tương ứng. Cần viết trực tiếp trong `baocao.docx`: PEAS cho Robot hút bụi, GridHunt, Lunar Lander + phân loại môi trường (quan sát đầy đủ/không đầy đủ, xác định/ngẫu nhiên, đơn/đa tác tử).

### Bài 2 – `robot_vacuum.ipynb`
Hiện là **template thuần túy của mhahsler** (110 điểm gốc, không phải thang điểm của đề Việt hóa), gồm các cell `# Your code goes here` chưa cài đặt:
- **Task 1** (cell-16, 18): môi trường phòng n×n, percept 4 bumper + dirty — **CHƯA LÀM**.
- **Task 2** (cell-20, 22): simple reflex agent — **CHƯA LÀM**.
- **Task 3** (cell-24, 25, 27): model-based reflex agent + mô tả state — **CHƯA LÀM**.
- **Task 4** (cell-29, 31): bảng so sánh 5×5/10×10/100×100, 100 lần chạy + biểu đồ — **CHƯA LÀM**.
- **Task 5, Advanced task, More Advanced**: câu hỏi mở rộng — PDF không yêu cầu bắt buộc nhưng notebook gốc có, cần quyết định có làm hay bỏ (phần "not for credit" có thể bỏ).

Lưu ý: PDF chỉ yêu cầu 3 loại agent + bảng kết quả 3 kích thước, khớp với Task 1–4 của notebook gốc. Phần "Advanced task" và "More Advanced" là phần điểm thưởng/không bắt buộc của bản gốc tiếng Anh — nhóm nên làm rõ với giảng viên xem có tính điểm không vì thang điểm PDF không nêu chi tiết.

### Bài 3 – `gridhunt_mini_assignment.ipynb`
Notebook có sẵn: `arena_environment()` hoàn chỉnh, monster ngẫu nhiên, hunter ngẫu nhiên mẫu (đã chạy thử với n=10, thất bại 52/100 lần).
- **Your Agent Implementation** (cell-22): hunter dùng `listen` để nhớ vị trí quái vật rồi đuổi theo — **CHƯA LÀM**.
- **Your Experiments** (cell-24): chạy ≥100 lần ở 10×10 và 30×30 — **CHƯA LÀM**.
- **Your Conclusion** (cell-26, 28): chiến lược, teleport, ảnh hưởng kích thước — **CHƯA LÀM**.

Khớp đúng yêu cầu PDF Bài 3.

### Bài 4 – `gridhunt_multiagent_mini_assignment.ipynb`
Môi trường 2 hunter cạnh tranh đã có sẵn (`arena_environment` hoàn chỉnh, đã demo random vs random: Hunter1 17 thắng, Hunter2 19 thắng, Monster 64 thắng).
- **Your Agent Implementation** (cell-21): agent đuổi bắt (không có `listen` — hunter luôn thấy vị trí quái vật) — **CHƯA LÀM**.
- **Your Experiments** (cell-23): 20×20 và ≥30×30, đấu với random hunter — **CHƯA LÀM**.
- **Your Conclusion** (cell-25, 27): chiến lược, teleport, kích thước — **CHƯA LÀM**.
- PDF còn yêu cầu thêm: **ít nhất 2 chiến lược hunter khác nhau so sánh trực tiếp**, và **đề xuất chuyển cạnh tranh → hợp tác** (chia sẻ vị trí quái vật) — đây là phần PDF bổ sung so với notebook gốc (notebook gốc để ở mục "More Work - optional"), nên **phải làm vì đề Việt hóa yêu cầu bắt buộc**.

### Bài 5 – `lunar_lander.ipynb`
Đã có: cài đặt Gymnasium, `random_agent_function` (baseline, đã chạy), `rocket_agent_function` (chỉ dùng động cơ chính) đã chạy 1000 episode: **reward TB -99.2, tỉ lệ thành công 4/1000** — rất kém, đúng như PDF mô tả cần cải tiến.
- **Implement A Better Reflex-Based Agent** (cell `6b6f4000`): dùng thêm động cơ trái/phải dựa trên vị trí ngang, vận tốc ngang, góc nghiêng — **CHƯA LÀM**.
- Thiếu: đánh giá trên 100 episode (PDF yêu cầu 100, code mẫu random dùng `n=1000` mặc định) — cần chỉnh về đúng 100 episode hoặc báo cáo rõ số lượng dùng.
- Thiếu: nhận xét độ ổn định + giải thích tại sao agent phản xạ luật cục bộ khó đạt hiệu năng cao trong môi trường liên tục — cần viết markdown.

## 3. Yêu cầu nộp bài (PDF mục 3 + yêu cầu bổ sung của giảng viên)

1. Hoàn thành mọi ô TODO/Your code, **không đổi chữ ký hàm** đã quy định (`agent_function(percepts...)`, `arena_environment(...)`, v.v.) — quan trọng vì môi trường gọi lại các hàm này.
2. Mỗi notebook phải **Run All** sạch từ đầu đến cuối, không lỗi, không log debug dài dòng.
3. Bổ sung markdown giải thích chiến lược, trạng thái nội bộ, bảng kết quả, biểu đồ so sánh.
4. Nộp `.ipynb` đã có output + báo cáo ngắn nhận xét/kết luận.
5. Sinh viên phải hiểu và giải thích được code khi được hỏi.
6. **Yêu cầu nộp bài bổ sung:**
   - Có file `baocao.docx` riêng: trang bìa + phân công công việc + giải thích các phần → **chưa tồn tại, cần tạo**.
   - Nén toàn bộ (notebook + báo cáo) thành `lab01_agent_hoten_masv.zip`, nhóm trưởng nộp.

## 4. Việc cần làm tổng hợp (checklist)

- [ ] Bài 1: Viết PEAS cho 3 bài toán trực tiếp vào báo cáo.
- [ ] Bài 2 (`robot_vacuum.ipynb`): môi trường n×n → simple reflex → model-based reflex → bảng/biểu đồ so sánh 5×5, 10×10, 100×100 (100 lần/cấu hình).
- [ ] Bài 3 (`gridhunt_mini_assignment.ipynb`): hunter model-based dùng `listen` → thử nghiệm 10×10 & 30×30 (≥100 lần) → tỉ lệ bắt + số bước TB → thảo luận teleport.
- [ ] Bài 4 (`gridhunt_multiagent_mini_assignment.ipynb`): ≥2 chiến lược hunter → đấu với random (100 lần) → 20×20 & ≥30×30 → đề xuất chuyển sang hợp tác.
- [ ] Bài 5 (`lunar_lander.ipynb`): agent dùng cả 3 động cơ (trái/phải/chính) → đánh giá 100 episode → reward TB + độ ổn định → giải thích hạn chế của reflex agent.
- [ ] Chạy lại toàn bộ Run All, dọn output debug thừa (đặc biệt notebook Lunar Lander đang in từng `Obs → Action` rất dài — cần tắt `verbose` khi chạy chính thức).
- [ ] Tạo `baocao.docx` (bìa, phân công, giải thích).
- [ ] Đóng gói `lab01_agent_hoten_masv.zip`.
