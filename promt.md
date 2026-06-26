Implement UI improvements ONLY for the Review History screen. Do NOT modify any other screen, component, shared styles, utility, or business logic. The Review Status screen should be used ONLY as the visual reference.

Requirements:

1. Match the overall premium look and feel of the Review Status screen while keeping all existing Review History functionality unchanged.

2. Improve the table layout:
   - Maintain consistent column spacing and alignment.
   - Increase readability with proper row height and padding.
   - Ensure header styling matches Review Status (background, typography, spacing, borders, and sticky header if already supported).

3. Borrower Name column:
   - Allow long borrower names to wrap onto the next line instead of being truncated or overflowing.
   - Keep the text left-aligned with proper vertical alignment.
   - Preserve consistent row spacing when text wraps.

4. Pagination section:
   - Redesign to match the premium Review Status layout.
   - Align "Showing X–Y of Z results", page navigation buttons, and "Rows per page" dropdown on a single horizontal line.
   - Ensure proper spacing between First, Previous, Next, and Last buttons.
   - Right-align the "Rows per page" dropdown.
   - Maintain responsive alignment on different screen sizes.

5. Rows Per Page dropdown:
   - Match the styling of the Bucket dropdown used in Review Status.
   - Keep consistent height, width, border, padding, font, and icon alignment.

6. Improve overall spacing:
   - Uniform margins around the table.
   - Consistent padding inside the card/container.
   - Proper alignment of filter/search area and pagination controls.

7. Responsive behavior:
   - No overlapping, clipping, or unnecessary horizontal scrolling.
   - Wrapped borrower names should not break the table layout.

8. Keep all existing functionality intact:
   - No changes to APIs.
   - No changes to data binding.
   - No changes to sorting, filtering, pagination logic, or business logic.

STRICT INSTRUCTION:
- Modify ONLY the Review History screen.
- Do NOT change Review Status or any other page.
- Do NOT modify shared/global components or styles.
- Keep the changes isolated to Review History UI only.
