# When retrieval helps versus stuffing the prompt

**Full-document prompting** works when the corpus is small enough to fit context limits and you are willing to pay the token cost. It avoids retrieval errors because the model sees everything—at least until the window fills up.

**Retrieval-augmented generation (RAG)** helps when you have **many documents**, **strict cost or latency budgets**, or **per-chunk access control** (only some passages may be shown to a given user).

**Rule of thumb:** if you can fit all relevant evidence in one prompt *affordably*, full context is simple and robust. If not, you retrieve a short evidence bundle first.
