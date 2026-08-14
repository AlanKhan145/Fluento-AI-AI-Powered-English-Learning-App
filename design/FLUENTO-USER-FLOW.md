# Fluento AI — User Flow & Screen Map

Tài liệu này là luồng UX bản đầu cho app học tiếng Anh cá nhân hoá bằng AI. Một lesson do AI tạo luôn bao gồm cả **Theory sections → Exercises → Results**; Grammar, Vocabulary và Writing là các section có thể được chọn trong cùng lesson, không phải những bài tách rời.

## 1. Luồng chính

```mermaid
flowchart TD
    A([Mở Fluento AI]) --> B{Đã có phiên đăng nhập?}
    B -- Có --> H[06 Home cá nhân hoá]
    B -- Chưa --> S[01 Welcome / Sign in]

    S --> C{Người dùng chọn gì?}
    C -- Sign in --> H
    C -- Google / Phone / Apple --> D[02 Nhập ngày sinh]

    D --> Dv{Ngày sinh hợp lệ?}
    Dv -- Chưa chọn hoặc lỗi --> D
    Dv -- Hợp lệ --> E[03 Nhập tên]
    E --> Ev{Tên hợp lệ?}
    Ev -- Trống hoặc quá dài --> E
    Ev -- Hợp lệ --> F[04 Chọn mục tiêu học]
    F --> Fv{Đã chọn ít nhất 1 mục tiêu?}
    Fv -- Chưa --> F
    Fv -- Đã chọn --> G[05 Chọn trình độ tiếng Anh]
    G --> Gv{Đã chọn trình độ?}
    Gv -- Chưa --> G
    Gv -- Đã chọn --> P[Lưu hồ sơ lần đầu]
    P --> H

    H --> X[07 Explore — tuỳ chọn]
    H --> I[08 Create lesson]
    H --> Y[19 Library]
    H --> Z[20 Profile]
    X --> I
    Y --> Y1[Saved lessons]
    Y --> Y2[Words you've learned]
    Y --> Y3[Grammar topics]
    Y --> Y4[Writing you've submitted]
    Z --> Z1[Edit profile / learning preferences]
    I --> Iv{Đủ topic, level, loại lesson và số câu?}
    Iv -- Chưa đủ --> I
    Iv -- Đủ --> J[AI đang tạo lesson]
    J --> Jv{Tạo thành công?}
    Jv -- Lỗi / timeout --> Jr[Hiện lỗi + Retry hoặc Edit]
    Jr --> I
    Jv -- Thành công --> K[09 Lesson ready<br/>Theory + Exercises]

    K --> TH[10 Theory overview]
    TH --> TG[11 Grammar theory<br/>(nếu chọn)]
    TG --> TV[12 Vocabulary theory<br/>(nếu chọn)]
    TV --> TW[13 Writing theory<br/>(nếu chọn)]
    TW --> EX[Exercise queue<br/>của cùng lesson]
    EX --> L[14 Multiple choice]
    L --> M{Còn exercise tiếp theo?}
    M -- True / False --> N[15 True / False]
    M -- Short answer --> O[16 Short answer]
    M -- Complete paragraph --> Q[17 Complete paragraph]
    M -- Writing task --> R[18 Writing + AI feedback]
    N --> M
    O --> M
    Q --> M
    R --> T([Hoàn thành lesson + lưu kết quả])
    M -- Hết exercise --> T
    T --> H
```

## 2. Thứ tự screen trong bộ reference

| ID | Screen | Vai trò | Dữ liệu cần lưu | CTA chính |
|---|---|---|---|---|
| 01 | Welcome / Sign in | Đăng nhập hoặc tạo tài khoản | auth provider | Continue / Sign in |
| 02 | Birthday | Hồ sơ lần đầu, riêng tư | `birthDate` | Continue |
| 03 | Name | Cá nhân hoá lời chào | `displayName` | Continue |
| 04 | Learning goal | Xác định nhu cầu học | `goals[]` | Continue |
| 05 | English level | Chọn độ khó khởi đầu | `level` | Start learning |
| 06 | Home | Điểm vào sau onboarding | profile + recent lessons | Create lesson |
| 07 | Explore | Khám phá lesson mẫu | filters / lesson cards | Open lesson / Create |
| 08 | Create lesson | Cấu hình yêu cầu cho AI | topic, type, level, count, exercise types | Generate lesson |
| 09 | Lesson ready | Xem tóm tắt trước khi học | generated lesson metadata | Start lesson |
| 10 | Theory overview | Mục lục các section của lesson | section order, completion | Start theory |
| 11 | Grammar theory | Description, formula, forms, uses, examples, notes | topicId, theory progress | Practice this topic |
| 12 | Vocabulary theory | Word, image/context, definition, example, word family | vocabulary items, saved words | Practice vocabulary |
| 13 | Writing theory | Task, structure, phrase bank, tone tips | writing prompt, rubric | Try writing task |
| 14 | Multiple choice | Exercise 1 | answer, correctness | Next |
| 15 | True / False | Exercise 2 | answer, correctness | Next |
| 16 | Short answer | Exercise 3 | free-text answer | Check / Next |
| 17 | Complete paragraph | Exercise 4 | paragraph answer | Check / Next |
| 18 | Writing + AI feedback | Chấm bài viết và phản hồi | score, corrections, model answer | Finish lesson |
| 19 | Library | Kho lesson, từ vựng, ngữ pháp và bài viết đã lưu | saved lessons, learned words, grammar topics, submissions | Open archive |
| 20 | Profile | Hồ sơ người học và learning preferences | avatar, goals, daily goal, reminders | Edit profile |

## 3. Quy tắc UX cho MVP

- Onboarding chỉ hiện một lần; nếu người dùng thoát giữa chừng, mở lại đúng bước cuối và giữ dữ liệu đã nhập.
- Nút Continue bị disable khi field bắt buộc chưa hợp lệ; lỗi hiển thị ngay dưới field, không chỉ dùng toast.
- `goals[]` cho phép chọn nhiều mục tiêu; `level` chỉ chọn một giá trị.
- Người dùng có thể sửa hồ sơ trong Profile sau onboarding.
- Library là kho học tập: mở lại lesson đã lưu, ôn từ vựng đã học, xem lại grammar topics và các bài writing đã nộp.
- Profile là nơi chỉnh avatar, learning goals, daily goal, focus topics và study reminders; không thay thế hồ sơ bắt buộc trong onboarding.
- Create lesson phải hiển thị rõ lesson sẽ có bao nhiêu theory sections và exercises; người dùng chọn các section Grammar/Vocabulary/Writing trong cùng form.
- Mỗi lesson phải đi qua Theory trước Practice; CTA ở cuối mỗi theory section mở bài tập tương ứng, còn lesson-level CTA cho phép tiếp tục section kế tiếp.
- Create lesson phải hiển thị trạng thái loading, retry và edit request khi AI lỗi.
- Lesson giữ tiến độ từng exercise; người dùng quay lại app có thể resume lesson chưa xong.
- Kết quả writing cần phân biệt điểm AI, sửa lỗi, giải thích và model answer.

## 4. Hợp đồng nội dung cho một lesson

```text
Lesson
├── metadata: title, level, estimatedMinutes
├── sections[]
│   ├── type: grammar | vocabulary | writing
│   ├── theory: description, formula/forms/uses hoặc word card hoặc writing structure
│   └── exercises[]: multiple-choice, true-false, short-answer, fill-in, writing
└── result: score, explanations, corrections, nextRecommendation
```

Mẫu `destination-b1-learning-web/data/lessons/unit-1.json` cung cấp trực tiếp các trường `topics`, `forms`, `uses`, `examples`, `notes` và `review`; Fluento dùng cùng ý tưởng này nhưng gom theory và exercise vào một lesson AI duy nhất.

## 5. Tài liệu học được áp dụng

- UX Design Roadmap — Module 04: target users, personas, product và competitor context.
- UX Design Roadmap — Module 05: product backlog, user stories và simple flowchart.
- UX Design Roadmap — Module 06: wireframe, layout rules và prototype.
- Product Manager Roadmap — Module 06: usability test, feedback và iteration.
- Build a Design System — dùng sau khi flow và wireframe ổn định để tạo tokens/components, không dùng để quyết định MVP flow.

## 6. Câu hỏi còn mở trước khi đưa vào code

1. Có cần age gate/parental consent cho người dùng dưới 13 tuổi không?
2. Level là tự chọn hay có bài placement test sau onboarding?
3. Một lesson được chọn một hay nhiều loại exercise?
4. Lesson lỗi có lưu draft request không, và giới hạn retry/quota là gì?
5. Có cần bắt buộc birthday thật hay chỉ cần năm sinh để tính nhóm tuổi?
