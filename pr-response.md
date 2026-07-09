# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** I changed the save_to_watchlist(user_id, film_id) function in services/watchlist_service.py to add_to_watchlist(user_id, film_id). I also changed the call sites in routes/watchlist/watchlist.py.
**How I verified:** I verified this change by restarting the flask server since it got crashed due to the ImportError at routes/watchlist/watchlist.py. After I reran the server, I did not get any errors or crashed out.

## Comment 2 — Deduplication
**What I did:**
**How I verified:**

## Comment 3 — Missing test
**What I did:**
**How I verified:**

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->


## Notes (Delete Later)

- File summary: Give the AI a file's full content and ask: "Summarize what this file is responsible for, what its main functions do, and what other parts of the codebase it depends on." Run this on models.py and the collection service file before reading the comments.
- Function explanation: Give the AI a function and ask: "What does this function do? Walk me through what happens at each step, and what it returns if the film_id doesn't exist." Use this on add_to_collection() before implementing your own deduplication in Comment 2.
- Test structure: Give the AI the test_collection.py file and ask: "What pattern does each test follow? What do I need to provide to write a test in the same style?" This will help you move faster on Comment 4.

6 PR Comments:
1. I notice watchlists default to public=True. We don't have a documented decision on default visibility for user lists. Before I can approve this, I need you to add a note to your PR description explaining your reasoning. I want to make sure we're being intentional here, not just inheriting a default.
    - Written response (specifically updating the PR description) + Decision
    - The reviewer wants you to be intentional about architectural decisions. You need to think through why you chose this default, document it in the PR description, and respond to the comment.
2. Please add a test for the case where film_id doesn't exist in the database. Look at the existing tests in test_collection.py — the pattern is there.
    - Code change (adding a new test function)
    - This establishes a pattern of alignment. The reviewer explicitly tells you to look at test_collection.py and mimic how missing items are tested there.
3. save_to_watchlist() should follow the project's naming convention. Compare with add_to_collection() — the pattern here is verb_to_noun. Please rename to add_to_watchlist() and update all call sites.
    - Code change (refactoring code and updating all call sites)
    - This establishes a naming pattern (verb_to_noun). For any future functions you write in this feature, you must follow this exact convention (e.g., remove_from_watchlist, not delete_watchlist_item).
4. What happens if a user calls this with a film that's already on their watchlist? The current implementation would add a duplicate entry. Please handle this case.
    - Code change (backend logic update)
    - This highlights a defensive programming pattern. You need to ensure the database layer or service layer handles bad user input or repetitive actions gracefully instead of creating duplicate/corrupted data.
5. I'd prefer watchlists to default to "date added" order rather than alphabetical. Most users want to see what they added recently. I'm open to discussion if you see it differently — but let's make a decision and document it.
    - Decision + Code change + Written response (documentation)
    - Similar to Comment #1, this is about user experience. They prefer a default but are open to discussion. You need to either accept their preference and change the .order_by() logic, or reply with a strong counter-argument, make a final decision, and ensure it's documented.
6. A refactor merged to main that changed film IDs from integers to UUIDs. Your watchlist code still references integer IDs. Please rebase on main and update accordingly.
    - Git action (rebase) + Code change (data type migration)
    - This is a maintenance pattern. When working in a team, main moves fast. You'll always need to sync with main and adapt your new feature branch to match the structural changes (like changing type hints or model schemas from int to UUID) made by other developers.
