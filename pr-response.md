# **PR Response Doc — CineLog Watchlist Feature**

### **AI Usage**
<!-- Fill in at the end — how you used AI tools during this project -->

--- 

### **Comment 1 — Rename**
**What I did:** I renamed the service function `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` so that it follows the project's existing verb-to-noun naming convention used throughout the codebase (for example, `add_to_collection()` and `remove_from_collection()`). I also updated every location where the function was referenced, including the import statement and function call in `routes/watchlist.py`.

**How I verified:** After completing the rename, I used a project-wide search for `save_to_watchlist` to confirm there were no remaining references anywhere in the repository. The search returned no results, confirming that all call sites had been updated. I then ran the full test suite using `pytest tests/ -v` to verify that the rename did not introduce any regressions or break existing functionality.

---

### **Comment 2 — Deduplication**
**What I did:**
**How I verified:**

---

### **Comment 3 — Missing test**
**What I did:**
**How I verified:**

---

### **Comment 4 — Default visibility**
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

---

### **Comment 5 — Sort order**
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

---

### **Comment 6 — Rebase**
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

---

### **PR Description**
<!-- Written at the end — feature overview, design decisions, manual testing steps -->