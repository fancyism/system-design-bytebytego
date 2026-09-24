# system-design-bytebytego

Agent skill — เวิร์กโฟลว์ออกแบบระบบสเกลสูงสไตล์ ByteByteGo ครบวงจร ตั้งแต่ pattern library จนถึง ADR

## ทำอะไร / What it does

ออกแบบ distributed system แบบมีระเบียบ: เลือก pattern จาก pattern library, ประเมิน scalability/reliability ด้วย rubric, วาด diagram ตาม playbook, และจบด้วยเอกสาร architecture decision record พร้อม trade-off ครบ

## ใช้เมื่อไหร่ / When to use

ออกแบบ backend/API gateway/queue/cache/real-time, ประเมิน fault tolerance และ observability, หรือต้องการ interview-ready diagram และเอกสารเลือกใช้เทคโนโลยี

## ติดตั้ง / Install

ใช้ได้กับ agent ที่รองรับ skills (Claude Code, Codex, OpenCode, ฯลฯ):

```bash
npx skills add fancyism/system-design-bytebytego
```

หรือคัดลอกโฟลเดอร์นี้ไปไว้ใน skill directory ของ agent คุณ (เช่น `~/.claude/skills/` หรือ `~/.agents/skills/`) แล้วเปิด session ใหม่

## ไฟล์ใน repo

- `SKILL.md` — ตัวเวิร์กโฟลว์หลัก
- `references/` — pattern library, scaling & reliability rubric, diagram playbook, output template, workflow, source notes
- `agents/openai.yaml`

> ได้แรงบันดาลใจจาก [ByteByteGoHq/system-design-101](https://github.com/ByteByteGoHq/system-design-101) — เนื้อหาเขียนขึ้นใหม่ทั้งหมดเป็นเวิร์กโฟลว์ใช้งานจริงกับ agent


## License

[MIT](./LICENSE) — © 2026 Affan Samaeng
