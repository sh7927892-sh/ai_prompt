# Security Specification: 나의 AI 프롬프트 창고 (`prompts`)

## 1. Access Control & Data Invariants
- Only `/prompts/{promptId}` is publicly readable (`get`, `list`); all other collections and documents are denied for both read and write.
- Only the verified admin (`request.auth.token.email == 'sh7927892@gmail.com' && request.auth.token.email_verified == true`) may `create`, `update`, or `delete` documents in `/prompts/{promptId}`.
- Every document in `/prompts/{promptId}` must have a valid document ID (`^[a-zA-Z0-9_\-]+$`, length 1–128).
- Every prompt document must contain strictly `['title', 'category', 'content', 'createdAt']` with no shadow or extra fields.
- `title` must be a non-empty string up to 100 characters.
- `category` must strictly be one of `['사진 합성', '스타일 변환', '캐릭터·포스터']`.
- `content` must be a non-empty string up to 10,000 characters (supporting multi-line prompts > 1,000 chars).
- `createdAt` must equal `request.time` on creation and remain immutable on update.
- No PII is collected or stored in Firestore.
