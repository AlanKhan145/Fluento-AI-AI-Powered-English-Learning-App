# Fluento AI — Ordered Reference Screens

Bộ này sắp theo một hành trình hoàn chỉnh: onboarding → hồ sơ lần đầu → Home → tạo lesson → làm bài → AI feedback.

## Screen order

1. `01_welcome_signin.png` — Welcome / Sign in
2. `02_birthday.png` — Birthday (generated reference)
3. `03_name.png` — Name (generated reference)
4. `04_learning_goal.png` — Learning goal (generated reference)
5. `05_english_level.png` — English level (generated reference)
6. `06_home.png` — Home
7. `07_explore.png` — Explore
8. `08_create_lesson.png` — Create lesson
9. `09_lesson_ready.png` — Lesson ready
10. `10_multiple_choice.png` — Multiple choice
11. `11_true_false.png` — True / False
12. `12_short_answer.png` — Short answer
13. `13_complete_paragraph.png` — Complete paragraph
14. `14_writing_ai_feedback.png` — Writing + AI feedback

Các ảnh có hậu tố “generated reference” được tạo bằng image generation tool theo style của ba ảnh onboarding người dùng cung cấp: dark grain, editorial serif heading, rounded controls và gradient pink–orange CTA. Các screen còn lại được giữ từ bộ `fluento-ai-v1`.

Luồng chi tiết và Mermaid diagram nằm ở [FLUENTO-USER-FLOW.md](../../FLUENTO-USER-FLOW.md).

> Lưu ý: bộ đầy đủ có cả theory và exercise nằm tại [fluento-ai-lesson-flow](../fluento-ai-lesson-flow). Thư mục hiện tại giữ các bản onboarding/lesson thử nghiệm trước đó.
