---
name: ux-master
description: Frontend UX verification skill — review UI components and pages against a comprehensive UX checklist covering 30 element types (buttons, forms, modals, accessibility, responsive design, and more). Use when reviewing frontend code, auditing UI components, or verifying that a PR meets UX standards. Triggers on tasks involving UI review, component audit, frontend code review, UX verification, or accessibility checks.
license: MIT
metadata:
  author: tuananhduong
  version: "1.0.0"
---

# FE-UX-Master: Frontend UX Verification

Verify frontend code against a comprehensive UX checklist covering buttons, links, inputs, forms, modals, navigation, accessibility (WCAG 2.2 AA target), responsive design, keyboard support, and more.

## When to Apply

Reference these guidelines when:
- Reviewing frontend PRs for UX quality
- Auditing UI components before release
- Building new UI elements from scratch
- Performing accessibility audits
- Checking responsive/mobile behavior
- Verifying loading, empty, and error states

## How to Use

When asked to verify or review frontend code, systematically check each relevant element type against its checklist below. For each violation found:

1. **Identify** the element and the specific rule violated
2. **Explain** why it's a problem for users
3. **Show** the current problematic code
4. **Provide** the corrected code or recommendation
5. **Rate severity**: 🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low

## 1. General Standards (applies to ALL elements)

Every component must satisfy these 7 criteria groups:

| Group | Standard |
|---|---|
| Recognizable | Users understand the element's function without trial |
| Operable | Adequate size, reasonable spacing, hard to mis-click |
| Responsive Feedback | Has hover, focus, active, loading, success, error states |
| Consistent | Same function = same appearance and behavior |
| Accessible | Usable via keyboard, screen reader, adequate contrast |
| Responsive | Works on mobile, tablet, and desktop |
| Error-proof | Prevents user errors and guides correction |

Minimum target: **WCAG 2.2 Level AA**.

## 2. Button

### Verification Checklist
- [ ] Label describes the action: `Save changes`, `Create account`, `Delete product`
- [ ] Avoids vague labels like `OK`, `Agree`, `Continue` without context
- [ ] Only one primary action per area is prominent
- [ ] Color is not the sole indicator of state
- [ ] Icon-only buttons have tooltip AND accessible name
- [ ] Dangerous buttons clearly convey the destructive action
- [ ] Uses semantic `<button>`, NOT `<div onClick>`
- [ ] Operable with `Enter` and `Space`
- [ ] Toggle buttons provide state via `aria-pressed`
- [ ] Button opening a modal moves focus into the modal

### Required States
- [ ] Default
- [ ] Hover
- [ ] Focus-visible
- [ ] Active/pressed
- [ ] Disabled
- [ ] Loading (prevents double-click, maintains width, shows status text)
- [ ] Success or error (when applicable)

### Loading State Rules
- [ ] Prevents repeated clicks
- [ ] Maintains original width (no layout shift)
- [ ] Shows status text like `Saving…`
- [ ] Does not disable entire UI unnecessarily

### Correct Pattern
```html
<button type="submit" disabled={isSubmitting}>
  {isSubmitting ? "Saving…" : "Save changes"}
</button>
```

## 3. Link

Links navigate; buttons perform actions. Never confuse them.

### Verification Checklist
- [ ] Link text describes the destination
- [ ] Avoids `Read more`, `Click here` when lacking context
- [ ] Inline links have underline or other indicator beyond color
- [ ] Links opening new tabs warn users when unpredictable
- [ ] External links may use icon but not excessively
- [ ] NEVER use `<a href="#" onClick={action}>` — use `<button>` instead

### Wrong
```html
<a href="#" onClick={saveData}>Save</a>
```

### Correct
```html
<button onClick={saveData}>Save</button>
```

## 4. Text Input

### Verification Checklist
- [ ] Always has a visible label outside the input
- [ ] Placeholder is example only, not a label substitute
- [ ] Label describes data, not the operation
- [ ] Correct `type`, `inputMode`, and `autocomplete` are set
- [ ] Does not clear user data on validation failure
- [ ] Width roughly reflects expected data length
- [ ] States covered: empty, filled, hover, focus, disabled, read-only, error, success (when needed)

### Error Messages Must Answer:
1. Where is the error?
2. Why did it happen?
3. How does the user fix it?

❌ Bad: `Invalid data.`

✅ Good: `Password must be at least 8 characters.`

- [ ] Error appears near the field, not just red border

### Correct Pattern
```html
<label for="phone">Phone number</label>
<input
  id="phone"
  type="tel"
  inputmode="tel"
  autocomplete="tel"
/>
```

## 5. Textarea

In addition to input standards:

- [ ] Allows vertical resize (unless layout has clear reason to lock)
- [ ] Shows character limit BEFORE user exceeds it
- [ ] Not too short for long content
- [ ] Preserves content on submit error
- [ ] Auto-grow has max-height limit if implemented
- [ ] For important content, consider autosave or leave-page warning

Example counter: `186/500 characters`

## 6. Select, Dropdown & Combobox

### Choose the Right Control
- < 5 options → consider radio buttons
- Medium list → native `<select>`
- Long list or needs search → combobox/autocomplete
- Multi-select → multi-select or checkbox list
- Actions → use menu, not dropdown

### Verification Checklist
- [ ] Has clear label
- [ ] Default value is not accidentally treated as user choice
- [ ] Placeholder (`Choose country`) is not a valid value
- [ ] Type-to-search for long lists
- [ ] Dropdown doesn't close prematurely during multi-select
- [ ] Selected state is easily recognizable
- [ ] Supports Arrow Up, Arrow Down, Enter, Escape

## 7. Checkbox

For 0, 1, or multiple independent selections.

### Verification Checklist
- [ ] Label is clickable
- [ ] No standalone checkbox without label
- [ ] Supports states: unchecked, checked, indeterminate, disabled, focus
- [ ] Does NOT auto-submit on change if consequences are significant
- [ ] Consent checkboxes are NOT pre-checked
- [ ] "Select all" handles indeterminate state correctly

### Correct Pattern
```html
<label>
  <input type="checkbox" name="newsletter" />
  Receive weekly newsletter
</label>
```

## 8. Radio Button

For exactly one choice from a group.

### Verification Checklist
- [ ] Choices are mutually exclusive
- [ ] Has group label via `<fieldset>` + `<legend>`
- [ ] All options visible when count is small
- [ ] Not used for on/off toggles (use switch/toggle)
- [ ] Default selection is safe and reasonable
- [ ] Arrow key navigation supported

### Correct Pattern
```html
<fieldset>
  <legend>Delivery method</legend>
  <label>
    <input type="radio" name="delivery" value="standard" />
    Standard delivery
  </label>
</fieldset>
```

## 9. Switch / Toggle

For settings with near-immediate effect.

### Verification Checklist
- [ ] Label describes state/feature, NOT two opposing actions
- [ ] On/off state is recognizable beyond color alone
- [ ] Change takes effect immediately
- [ ] If "Save" button is needed, checkbox is usually better
- [ ] Has accessible state via `role="switch"` and `aria-checked`

## 10. Form

### Structure
- [ ] Long forms divided into sections with headings
- [ ] Field order follows user logic, not database schema
- [ ] Only truly required data is requested
- [ ] Optional fields marked with `(Optional)`
- [ ] Doesn't ask for already-provided information
- [ ] One field error does NOT reset entire form

### Validation
- [ ] Validates on blur or submit
- [ ] Does NOT show error on first keystroke
- [ ] Error summary at top for long forms
- [ ] Focus moves to summary or first error field on failed submit
- [ ] Critical transactions offer review or undo step

### Submit
- [ ] Prevents duplicate submissions
- [ ] Shows processing state
- [ ] Clear success notification
- [ ] No abrupt page transitions without feedback
- [ ] Preserves entered data on server error

## 11. Modal / Dialog

- [ ] Clear title
- [ ] Easy-to-find close button
- [ ] `Escape` closes (except for critical mandatory flows)
- [ ] Backdrop click closes for non-destructive actions
- [ ] Backdrop click does NOT close if data loss risk
- [ ] Focus trapped inside modal when open
- [ ] Focus returns to trigger element on close
- [ ] Content behind modal is non-interactive
- [ ] No modal stacking (modal on modal)
- [ ] On mobile, long content → consider full-screen or separate page

## 12. Alert, Toast & Notification

| Type | When to Use |
|---|---|
| Inline message | Feedback for a specific area |
| Toast | Brief action confirmation, no action needed |
| Banner | Important page-level info |
| Alert dialog | User must decide before continuing |

### Toast Checklist
- [ ] NOT used for errors requiring user correction
- [ ] Short content, clear result
- [ ] Doesn't disappear too quickly
- [ ] Close button for long-lived toasts
- [ ] Action like `Undo` when appropriate
- [ ] Not too many toasts at once
- [ ] Screen reader receives status message

## 13. Tooltip

- [ ] Contains supplementary, NOT essential, information
- [ ] Shows on hover AND keyboard focus
- [ ] Dismissible with `Escape`
- [ ] Does not cover the target element
- [ ] No forms or complex actions inside
- [ ] Short content
- [ ] Icon-only buttons have accessible name regardless of tooltip

## 14. Tabs

- [ ] Active tab is clearly distinguishable
- [ ] Color is not the only active indicator
- [ ] Tab order is logical
- [ ] Arrow key navigation between tabs
- [ ] Each tab correctly linked to its panel
- [ ] Not used when users need to compare content across tabs
- [ ] On mobile, tab row does not overflow awkwardly

## 15. Accordion

- [ ] Header is a `<button>`
- [ ] Entire header area is clickable
- [ ] Expand/collapse icon present
- [ ] Provides `aria-expanded`
- [ ] Keyboard accessible
- [ ] Critical information not hidden inside
- [ ] Not deeply nested (avoid >2 levels)

## 16. Navigation & Menu

- [ ] Navigation position consistent across pages
- [ ] Current page clearly marked
- [ ] Labels are short and understandable
- [ ] Not too many menu levels
- [ ] Dropdown menus work on click (not hover-only)
- [ ] `Escape` closes menu
- [ ] Click outside closes menu
- [ ] Mobile menu preserves context on open/close
- [ ] Logo links to home (if a product convention)

## 17. Table

- [ ] Clear headers
- [ ] Left-align text; right-align numbers
- [ ] Consistent units
- [ ] Sort direction and column are visible
- [ ] Active filters are visible
- [ ] Empty state explains WHY no data
- [ ] NOT used for layout purposes
- [ ] Row actions are findable but not noisy
- [ ] Bulk actions only appear when rows are selected
- [ ] Mobile: horizontal scroll, card view, or prioritized columns

## 18. Pagination & Infinite Scroll

### When to Paginate
- Users need to know total pages
- Need to return to a specific position
- Administrative or lookup-oriented data
- URL needs to be shareable

### When to Infinite Scroll
- Content exploration is primary
- Footer access is not frequent
- Exact position recall is not needed

### Verification Checklist
- [ ] Filter, sort, and page are preserved in URL
- [ ] Browser back returns to correct state
- [ ] Loading skeleton or indicator present
- [ ] Scroll position preserved on back navigation
- [ ] Infinite scroll has `Load more` fallback

## 19. Card

- [ ] Clear hierarchy: image → title → metadata → action
- [ ] Not every element inside is a separate link
- [ ] Full-card click has proper semantics and clear focus
- [ ] No button inside a full-card link (`<a>` wrapping `<button>`)
- [ ] Cards in same list have consistent structure
- [ ] Long text doesn't hide important content
- [ ] Skeleton matches real layout (no layout shift)

## 20. Image, Icon & Avatar

### Image
- [ ] `alt` describes the image's purpose
- [ ] Decorative images use `alt=""`
- [ ] No important text embedded in images
- [ ] `width` and `height` declared
- [ ] Responsive images + proper lazy loading
- [ ] Hero images above the fold are NOT lazy-loaded

### Icon
- [ ] Uncommon icons have labels
- [ ] Icon-only actions have tooltip + accessible name
- [ ] Same icon doesn't mean different things
- [ ] Stroke, size, and alignment are consistent
- [ ] Decorative icons hidden from screen readers

### Avatar
- [ ] Fallback initials or placeholder
- [ ] Avatar is not the sole identifier
- [ ] In user lists, always show name alongside when possible

## 21. Loading State & Skeleton

- [ ] < ~300ms: no indicator needed
- [ ] Longer operations: clear feedback required
- [ ] Skeleton roughly reflects real content structure
- [ ] No full-page spinner for local operations
- [ ] Preserve old content during refresh when possible
- [ ] Long tasks: show meaningful progress
- [ ] Allow cancellation of lengthy operations

## 22. Empty State

Distinguish at least four cases:
1. Never had data
2. No search results
3. No data due to filter
4. Error loading data

### Empty state should include:
- [ ] What's happening
- [ ] Why there's no data
- [ ] What the user can do next
- [ ] A primary action when appropriate

## 23. Search

- [ ] Placeholder is descriptive: `Search by name or order number`
- [ ] Quick-clear button
- [ ] `Enter` triggers search
- [ ] Debounce for search-as-you-type
- [ ] No request per keystroke if unnecessary
- [ ] Query shown on results page
- [ ] Result count displayed
- [ ] Easy query modification
- [ ] Suggestions support arrow keys, Enter, Escape

## 24. Date Picker & Time Picker

- [ ] Keyboard input allowed
- [ ] Format shown in label or helper text
- [ ] Auto-format without unpredictable cursor jumps
- [ ] Invalid dates disabled with reason when needed
- [ ] Range picker clearly shows start and end
- [ ] Shortcuts: `Today`, `Last 7 days` when appropriate
- [ ] Respects locale and timezone
- [ ] Full date displayed where ambiguity is possible

## 25. File Upload

- [ ] Click AND drag-and-drop supported (not drag-and-drop only)
- [ ] Format, max size, and file count stated
- [ ] Validate before upload when possible
- [ ] Progress per file
- [ ] Retry and remove allowed
- [ ] Single file error doesn't clear entire list
- [ ] Dropzone is keyboard accessible

## 26. Responsive & Mobile UX

- [ ] No horizontal scroll except intentional areas
- [ ] Not hover-dependent
- [ ] Main content readable at 200% zoom
- [ ] Mobile keyboard doesn't cover active field
- [ ] Auto-scroll keeps field + error visible on focus
- [ ] Correct input keyboard type used
- [ ] Sticky header/footer don't take excessive viewport
- [ ] Safe area handled on notch devices
- [ ] Orientation not locked unless truly necessary

## 27. Keyboard & Focus

- [ ] `Tab` follows logical order
- [ ] `Shift+Tab` reverses correctly
- [ ] `Enter` and `Space` activate correct controls
- [ ] `Escape` closes modals, menus, popups
- [ ] Focus indicator always visible
- [ ] NO `outline: none` without replacement
- [ ] Focus not hidden behind sticky header/modal/overlay
- [ ] Dynamic content changes don't lose focus
- [ ] Skip link for pages with long navigation

### Correct CSS
```css
:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 3px;
}
```

## 28. Color & Typography

### Color
- [ ] Body text: minimum 4.5:1 contrast ratio
- [ ] Large text: minimum 3:1 contrast ratio
- [ ] UI controls/focus have clear contrast against background
- [ ] State is not communicated by color alone
- [ ] Checked in both light and dark mode
- [ ] Disabled is readable but distinguishable from enabled

### Typography
- [ ] Body text ≥ 16px
- [ ] Line-height ~1.4–1.6
- [ ] Line length ~45–75 characters
- [ ] Not too many font sizes and weights
- [ ] Headings follow hierarchy
- [ ] No long sentences in ALL CAPS
- [ ] Text reflows properly on zoom or increased font size

## 29. Animation & Motion

- [ ] Animation explains state change
- [ ] Common transitions: 150–300ms
- [ ] No delays to user action just for animation
- [ ] Avoid large, flashing, or parallax motion
- [ ] Supports `prefers-reduced-motion`

### Correct CSS
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    scroll-behavior: auto !important;
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

## 30. Component Definition of Done

A component is complete only when ALL are satisfied:

- [ ] All visual states implemented
- [ ] Correct semantic HTML used
- [ ] Works with mouse, touch, AND keyboard
- [ ] Has accessible name, role, and state
- [ ] Clear, logical focus order
- [ ] Works on mobile, tablet, desktop
- [ ] Doesn't break with long content or long translations
- [ ] Has loading, empty, error, and disabled states (where applicable)
- [ ] Doesn't depend solely on color or icon
- [ ] No significant layout shift
- [ ] Unit tests for critical logic
- [ ] Interaction tests for keyboard
- [ ] Screen reader verified for critical flows
- [ ] Meets WCAG 2.2 AA

---

## Quick Review Checklist

Before merging any UI, answer these 10 questions:

1. Does the user understand what this element does?
2. Does the user know what state the element is in?
3. Does the system respond immediately on interaction?
4. When there's an error, does the user know how to fix it?
5. Is it fully keyboard operable?
6. Does it work on small screens?
7. Are long text, empty data, and request errors handled?
8. Can the user undo or confirm dangerous actions?
9. Is state communicated beyond color alone?
10. Do back button, refresh, and deep link preserve state correctly?

---

## Verification Report Format

When verifying frontend code, produce a structured report:

```
## UX Verification Report

### Summary
- Elements checked: N
- Violations found: N (🔴 Critical: N | 🟠 High: N | 🟡 Medium: N | 🟢 Low: N)
- Compliance score: X%

### Critical Issues
[file:line] Rule violated — explanation + fix

### High Priority Issues
[file:line] Rule violated — explanation + fix

### Medium Priority Issues
[file:line] Rule violated — explanation + fix

### Low Priority Issues
[file:line] Rule violated — explanation + fix

### Accessibility Scorecard
- Keyboard navigation: Pass/Fail/Partial
- Screen reader: Pass/Fail/Partial
- Color contrast: Pass/Fail/Partial
- Semantic HTML: Pass/Fail/Partial
- Focus management: Pass/Fail/Partial

### Recommendations
1. ...
2. ...
```
