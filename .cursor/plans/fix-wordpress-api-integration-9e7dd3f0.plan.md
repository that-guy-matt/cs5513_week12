<!-- 9e7dd3f0-7cc4-4b10-adb8-e38a2bce07c7 d973b180-ce1e-421e-92a8-e5472009a953 -->
# Fix WordPress API Integration

## Issues Identified

1. **API Response Parsing**: Code expects `res.body.posts` but WordPress returns an array directly
2. **Field Name Mismatch**: WordPress uses `ID`, `post_title`, `post_date`, `post_content` instead of `id`, `title`, `date`, `content`
3. **Date Format**: WordPress returns "YYYY-MM-DD HH:mm:ss" but Date component needs ISO format
4. **Missing await**: `getAllPostIds()` is async but called without `await` in `getStaticPaths()`
5. **Content Field**: Post page expects `content` but WordPress returns `post_content`

## Implementation Plan

### 1. Fix `lib/posts.js`

- Change `res.body.posts` to `res.body` (response is array directly)
- Map WordPress field names to expected format:
- `ID` → `id`
- `post_title` → `title`
- `post_date` → `date` (convert to ISO format)
- `post_content` → `content`
- Remove unused `path` import
- Add error handling for empty responses

### 2. Fix `pages/posts/[id].js`

- Add `await` to `getAllPostIds()` call in `getStaticPaths()`
- Ensure `getStaticPaths` properly handles async function

### 3. Update Date Conversion

- Convert WordPress date format "YYYY-MM-DD HH:mm:ss" to ISO format "YYYY-MM-DDTHH:mm:ss" for date-fns compatibility

### 4. Test Data Mapping

- Verify all required fields are mapped correctly for both list and detail views

### To-dos

- [ ] Update lib/posts.js to correctly parse WordPress API response and map field names (ID→id, post_title→title, post_date→date, post_content→content)
- [ ] Convert WordPress date format (YYYY-MM-DD HH:mm:ss) to ISO format for date-fns compatibility
- [ ] Add await to getAllPostIds() call in pages/posts/[id].js getStaticPaths() function
- [ ] Ensure post_content is mapped to content field for post detail page rendering