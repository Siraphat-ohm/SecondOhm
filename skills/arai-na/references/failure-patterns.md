# Failure patterns

Calibration reference for `arai-na`. Each pattern passes the letter of the rules while failing the goal; check a drafted rewrite against these before sending it.

## Translation-only

Before: `Move validation to the boundary so internal code can rely on the invariant.`

Fails — Thai words, same abstraction, no more familiar than the English:
> ย้ายการตรวจสอบไปที่ขอบเขต เพื่อให้ code ภายในพึ่งพาค่าคงสภาพได้

Closes the gap by grounding the terms in what they do, not translating them:
> boundary ในที่นี้คือจุดที่ข้อมูลจากภายนอกเข้าระบบ ถ้าเราตรวจข้อมูลตรงทางเข้านี้ก่อน code ด้านในจะไม่ต้องรองรับ input ที่ผิดทุกจุด — เงื่อนไขที่ code ด้านในถือว่าเป็นจริงเสมอ เช่น `user.id` ต้องมีค่า นี่แหละคือ invariant

## Over-restart

User loses one step in an explanation about React re-renders.

Fails — the missing step didn't require re-teaching the framework:
> ก่อนอื่นเรามาทำความเข้าใจกันว่า React คืออะไร...

Start from the nearest point still understood instead: what re-renders, why, and only then the one skipped mechanism.

## False simplicity

Fails — drops the condition that makes the claim true:
> พูดง่ายๆ คือ cache ทำให้ทุกอย่างเร็วขึ้น

Keeps it:
> cache ช่วยลดงานซ้ำเมื่อเราใช้ผลเดิมได้ มันจึงเร็วขึ้นเฉพาะกรณีที่ reuse ผลเดิมถูกต้องและ cache hit บ่อยพอ
