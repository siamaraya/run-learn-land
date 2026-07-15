# วังหมีกระทิง Run Learn Land — Project Hub

Family Nature Trail Festival · 25–27 ธันวาคม 2569 · วังหมี กระทิง

- **[เปิดหน้า Project Hub](https://siamaraya.github.io/run-learn-land/)** — ภาพรวมงาน, งานค้าง, ฝ่ายงาน 12 ฝ่าย, แผนการเงิน, ไทม์ไลน์ประชุม
- [slides-content.md](slides-content.md) — สรุปเนื้อหาจากสไลด์ประชุมแบบจัดโครงสร้าง

> ข้อมูลสรุปจาก Google Slides ประชุมทีม

## Auto-sync pipeline

GitHub Action ([sync-slides.yml](.github/workflows/sync-slides.yml)) ตรวจสไลด์ทุก 30 นาที —
ถ้าเนื้อหาเปลี่ยน Claude จะเรียบเรียงการเปลี่ยนแปลงลง `index.html` + `slides-content.md`
แล้ว commit อัตโนมัติ หน้าเว็บอัปเดตเองไม่ต้องแตะ

- Baseline: [data/slides.txt](data/slides.txt) · Prompt: [scripts/sync-prompt.md](scripts/sync-prompt.md)
- ต้องมี secret `CLAUDE_CODE_OAUTH_TOKEN` (จาก `claude setup-token`) หรือ `ANTHROPIC_API_KEY`
- สั่งรันทันทีได้จากแท็บ Actions → "Sync slides to dashboard" → Run workflow
