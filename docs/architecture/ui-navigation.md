# UI & Navigation

Shell structure
- AppShell routes (suggested):
  - /home (Home page: Browse Library, Add to Library)
  - /library (Library view by collection)
  - /collections (manage collections)
  - /add (Add / autocomplete flow)
  - /game/{id} (Game detail / edit)
  - /settings

Home screen
- Two main actions: "Browse Library" and "Add to Library".

Library UI
- Use `CollectionView` (adaptive) with a grid on wider screens and a single column on phones.
- Item template: thumbnail (88dp), Title, short description (single line with ellipsis), small metadata (players/time).

Add flow
- Search/autocomplete box that queries local DB and shows up to 3 suggestions.
- If user selects suggestion, open detail screen (with option to save to collection).
- If not found, allow "Create new game" flow where user can fill full fields.

Game detail & edit
- Show full data (thumbnail top, title, year, players, ratings, full description, tags, notes)
- Editing: open separate edit screen for full edits. Allow inline quick edits for status or play count.

Design choices
- Use Material defaults for Android; follow platform conventions for spacing and button placement.
- Keep actions prominent: floating action button for Add on library screens.
