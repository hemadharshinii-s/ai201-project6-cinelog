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
**What I did:** I created a new test file, `tests/test_watchlist.py`, and added a test named `test_add_to_watchlist_nonexistent_film_raises()`. This test verifies that attempting to add a film whose `film_id` does not exist in the database raises `FilmNotFoundError` instead of resulting in a database integrity error or creating an invalid watchlist entry.

To keep the testing style consistent with the rest of the project, I modeled the test directly after `test_add_to_collection_nonexistent_film_raises()` in `tests/test_collection.py`. I used the same fixture structure, application context, and assertion pattern, changing only the service function being tested.

**How I verified:** I first ran `pytest tests/test_watchlist.py -v` to confirm that the new test passed independently. I then ran the full test suite using `pytest tests/ -v` to verify that the new test integrated cleanly with the existing tests and did not introduce any regressions.

---

### **Comment 4 — Default visibility** 
**My position:** I support keeping the default value of `public=True` for watchlists.

**Reasoning:** CineLog is a social platform centered around discovering and discussing films. A public watchlist allows users to share upcoming movies they are interested in, helps other users discover new films through recommendations, and makes user profiles more engaging without requiring additional configuration. Most users who choose to create a watchlist are doing so to organize films they plan to watch, and making those lists visible by default encourages discovery and community interaction while still allowing future privacy features to build on that foundation.

**Tradeoff acknowledged:** The primary advantage of a private-by-default watchlist is that it prioritizes user privacy and avoids exposing viewing interests unless a user explicitly chooses to share them. That approach reduces the possibility of unintentionally revealing personal preferences. However, for CineLog's emphasis on film discovery and social engagement, I believe a public default provides greater value for the majority of users. If stronger privacy controls become a priority in the future, allowing users to change the visibility after creating the watchlist would preserve that flexibility without sacrificing discoverability by default.

---

### **Comment 5 — Sort order**
**My position:** I agree with changing the default watchlist order to sort by date added (newest first).

**Reasoning:** A watchlist primarily represents a user's future viewing queue rather than a permanent catalog. In most cases, users interact with the films they have added most recently, whether they are reviewing recent recommendations or deciding what to watch next. Displaying the newest additions first makes those recently saved films immediately visible and reduces the need to search through older entries.

**Engagement with reviewer's point:** I agree with the reviewer's observation that most users expect to see what they added recently. While alphabetical ordering makes it slightly easier to locate a specific title in a very large watchlist, watchlists are generally revisited based on when films were discovered rather than by title. If CineLog later supports user-selectable sorting, alphabetical order could become an optional view, but I believe date-added is the better default because it aligns with the most common workflow for managing a watchlist.

---

### **Comment 6 — Rebase**
**What conflicted:** I fetched the latest `main` branch and rebased my `feature/watchlist` branch onto it. During the rebase, Git reported a merge conflict in `.gitignore` because both branches had added the file independently. After completing the rebase, I also discovered that the updated `main` branch no longer contained the `WatchlistEntry` model, causing an import error when running the tests. This was a consequence of rebasing onto the UUID refactor, which changed the underlying models.

**How I resolved it:** I resolved the `.gitignore` conflict by keeping the combined ignore entries, staged the file, and continued the rebase with `git rebase --continue`. After the rebase completed, I restored the `WatchlistEntry` model in `models.py` and updated it to use UUID-based film IDs (`db.String(36)`) so it matched the refactored `Film` model and remained compatible with the rest of the application.

**How I verified no conflict remains:** After resolving the conflicts, I ran the full test suite with `pytest tests/ -v` and confirmed that all five tests passed successfully. I also reviewed the commit history using `git log --oneline --decorate` to verify that my branch has a linear history with no feature-branch merge commits.

---

### **PR Description**
<!-- Written at the end — feature overview, design decisions, manual testing steps -->