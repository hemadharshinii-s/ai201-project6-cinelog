# **PR Response Doc — CineLog Watchlist Feature**

### **AI Usage**
<!-- Fill in at the end — how you used AI tools during this project -->

--- 

### **Comment 1 — Rename**
**What I did:** I renamed the service function `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` so that it follows the project's existing verb-to-noun naming convention used throughout the codebase (for example, `add_to_collection()` and `remove_from_collection()`). I also updated every location where the function was referenced, including the import statement and function call in `routes/watchlist.py`.

**How I verified:** After completing the rename, I used a project-wide search for `save_to_watchlist` to confirm there were no remaining references anywhere in the repository. The search returned no results, confirming that all call sites had been updated. I then ran the full test suite using `pytest tests/ -v` to verify that the rename did not introduce any regressions or break existing functionality.

---

### **Comment 2 — Deduplication**
**What I did:** I added a duplicate check to `add_to_watchlist()` so that the service verifies whether the specified film is already on the user's watchlist before creating a new `WatchlistEntry`. If an existing entry is found for the same `user_id` and `film_id`, the function now raises an `AlreadyInWatchlistError` instead of creating a duplicate record. I also documented this behavior in the function's `Raises` section.

To keep the implementation consistent with the rest of the codebase, I followed the same pattern used in `add_to_collection()` within `services/collection_service.py`: validate that the film exists, check for an existing entry, raise a custom exception if a duplicate is detected, and only create and commit a new entry when no duplicate exists.

**How I verified:** I compared my implementation against the existing `add_to_collection()` function to ensure it followed the same validation and deduplication workflow. After making the change, I ran the full test suite using `pytest tests/ -v` to confirm that the modification did not introduce any regressions.

---

### **Comment 3 — Missing test**
**What I did:** I created a new test file, `tests/test_watchlist`.py, and added a test named `test_add_to_watchlist_nonexistent_film_raises()`. This test verifies that attempting to add a film whose `film_id` does not exist in the database raises `FilmNotFoundError` instead of resulting in a database integrity error or creating an invalid watchlist entry.

To keep the testing style consistent with the rest of the project, I modeled the test directly after `test_add_to_collection_nonexistent_film_raises()` in `tests/test_collection.py`. I used the same fixture structure, application context, and assertion pattern, changing only the service function being tested.

**How I verified:** I first ran `pytest tests/test_watchlist.py -v` to confirm that the new test passed independently. I then ran the full test suite using `pytest tests/ -v` to verify that the new test integrated cleanly with the existing tests and did not introduce any regressions.

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