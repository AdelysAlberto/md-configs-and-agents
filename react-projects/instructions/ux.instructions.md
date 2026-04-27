# Master UX/UI Design System Prompts

This document contains specialized prompts designed to guide an AI in following best practices for UX/UI, based on core design principles and cognitive psychology.

---

## 1. Critical Actions: Bottom Sheets vs. Modals
**Context:** Mobile Confirmation for Irreversible Actions (e.g., Account Deletion).

**UX Prompt:**
> "Design a mobile interface for a critical confirmation action, such as 'Delete Account'. 
> **Core Constraint:** Use a native **Bottom Sheet** overlay instead of a centered modal. 
> **Rationale:** The bottom sheet must feel thumb-friendly and aligned with mobile native patterns. 
> **Visual Hierarchy:** > - Include a clear 'Drag Handle' at the top of the sheet.
> - The 'Delete' button must be the primary action in a high-contrast 'Danger Red'.
> - The 'Cancel' button should be secondary (ghost or neutral grey).
> - Ensure the background behind the sheet is dimmed (overlay) but allows the previous context to be slightly visible at the top."

---

## 2. Content Cards: Reducing Visual Noise
**Context:** Content Feed or Article Cards.

**UX Prompt:**
> "Design a content card for a mobile application ('Daily Reads' section). 
> **UX Rule:** Prioritize legibility by minimizing visual noise. 
> **Instructions:** > - Avoid underlined links within the text body. 
> - Use a clean typographic hierarchy: Bold headers and a lighter, well-spaced font for the body.
> - Ensure the text does not cut off mid-sentence; use elegant truncation (...) only if necessary.
> - Include a single, clear 'Call to Action' (CTA) button at the bottom (e.g., 'Explore') to focus the user's attention."

---

## 3. Decision Simplification: Hick’s Law
**Context:** User Feedback or Status Selection.

**UX Prompt:**
> "Create a mood-tracking or status-selection interface for a mobile app. 
> **UX Rule:** Apply Hick's Law to reduce cognitive load. 
> **Constraint:** Instead of a long dropdown menu with 8+ options, simplify the choice to 3 clear, distinct paths (e.g., 'Great', 'Okay', 'Bad'). 
> **Visual Style:** Use a clean layout with enough touch-target space for each option to prevent selection errors."

---

## 4. Search Experience: Visible Filters
**Context:** E-commerce Navigation.

**UX Prompt:**
> "Design the initial search/home screen for a clothing e-commerce app. 
> **UX Rule:** Don't force the user to type if they don't know what they want yet. 
> **Feature:** Display visible, horizontal 'Pill' filters (e.g., 'All', 'Men', 'Women', 'Kids') immediately below or above the search bar. 
> **Goal:** Provide 'paths' for navigation before requiring keyword input."

---

## 5. Trust-Based Checkout Flow
**Context:** Payment Process.

**UX Prompt:**
> "Design a multi-step checkout flow for a mobile app, treating the process as a 'conversation of trust'. 
> **Workflow:** > 1. **Payment Methods:** Present only the top 2-3 most used options (e.g., Card, PayPal).
> 2. **Data Entry:** Use clear input masks for credit card numbers (e.g., **** **** **** 1234).
> 3. **Feedback:** Include a 'Processing' screen with a progress bar or animation to reassure the user.
> 4. **Success:** A final screen confirming the purchase and explaining next steps (e.g., 'Check your email for the receipt')."

---

## 6. Meaningful Empty States
**Context:** 'No Results' Search Screen.

**UX Prompt:**
> "Design a 'No Results Found' screen for a recipe search app. 
> **UX Rule:** An empty state should never be a dead end. 
> **Elements:** > - A friendly illustration and a clear 'No results' message.
> - **Path Forward:** Provide a 'Recommended for you' or 'Trending' section below the error message to keep the user engaged and reduce frustration."

---

## 7. Efficient Dropdowns: Search & Identity
**Context:** Team Assignment or User Picker.

**UX Prompt:**
> "Design a user selection component for a task management app. 
> **UX Rule:** Avoid simple text-based long lists. 
> **Enhancements:** > - Integrate a search bar within the selection component.
> - Include user avatars (photos) next to names for instant visual recognition.
> - Group results by relevance or department (e.g., 'Related' or 'Frequent')."

---

## 8. Visual Hierarchy in Profile Cards
**Context:** Freelancer or Talent Directory.

**UX Prompt:**
> "Design a professional profile card for a freelancer marketplace. 
> **UX Rule:** Establish a clear 'Decision Map' via visual hierarchy. 
> **Structure:** > - **Top:** Small, clear avatar and Name.
> - **Sub-header:** Professional Role (e.g., 'Product Designer').
> - **Center:** Key metrics (Projects, Earnings, Rating) with balanced whitespace.
> - **Bottom:** Full-width CTA buttons (e.g., 'Hire', 'Message') to conclude the user's decision journey."

---

## 9. Frictionless Login
**Context:** Authentication Screen.

**UX Prompt:**
> "Design a 'Sign In' screen for a mobile application. 
> **UX Rule:** Combat decision fatigue by limiting social login options. 
> **Layout:** > - Display the 2 most relevant social login buttons (e.g., Google and Apple).
> - Hide additional options under a 'More options' or 'Other networks' progressive disclosure link.
> - Ensure clear, action-oriented microcopy (e.g., 'Continue with...' instead of just the logo)."