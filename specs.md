SPEC FILE

AgentHub — Product Overview
AgentHub is a B2B SaaS platform that rents AI agents to companies. An agent is a pre-configured intelligent assistant that can be equipped with modular skills — discrete capabilities such as web browsing, document reading, or calendar management — and deployed to handle specific business tasks. Clients rent agents under contracts that specify which skills are included, the rental period, and the total cost.

The Admin
The admin is an internal AgentHub operator (not a client). They use this panel to oversee the entire platform: monitoring revenue and financial losses from discounts, managing the user base, configuring and troubleshooting agents across all clients, maintaining the skills catalog, reviewing rental contracts, and resolving execution errors. The admin has a platform-wide view — they see every user, agent, contract, and error across all clients, rather than the scoped view an individual client would have.

Tech Stack & ConstraintsTech Stack & Constraints

The prototype is a static, front-end-only build with no backend and no build step. Everything runs directly in the browser from a single HTML file (or a small set of static files).

Markup: Plain HTML5. No templating engines or component frameworks.

Styling: Tailwind CSS, loaded via CDN (<script src="https://cdn.tailwindcss.com"></script>). No local Tailwind build, PostCSS, or config compilation — any customization is done inline via the CDN config object. Light/dark theming uses Tailwind's dark: utilities with class-based dark mode (darkMode: 'class').

Interactivity: Vanilla JavaScript only. No React, Vue, Svelte, Alpine, jQuery, or any other framework or library. All behavior — dropdowns, modals, skill expand/collapse, theme toggle — is implemented with native DOM APIs and event listeners.

Backend: None. There is no server, API, or database. All data shown in the panel is hardcoded mock data (in markup or JS), and all actions (delete, mark as resolved, etc.) operate only on the client side for demonstration purposes.

Persistence: None required. State lives in memory for the session; a page reload resets it.
Dependencies: Zero npm packages, no bundler (Webpack/Vite/etc.), no install step. The prototype is opened by loading the HTML file in a browser.

Section Specifications

1. Dashboard

Metric cards — Four cards in a responsive grid (2×2 on desktop, single column on mobile). Each card contains an icon, a text label, and a hardcoded value. The four metrics are: Total Revenue (this month), Discount & Coupon Losses, Active Agents, and Failing Agents. Each card uses a distinct accent color tied to its metric type and carries a subtle shadow. Cards have no interactive behavior beyond a slight hover elevation.

Accent color coding — Revenue uses a positive/green accent, Losses a warning/amber accent, Active Agents a neutral/blue accent, Failing Agents a danger/red accent. The accent appears on the icon and/or a colored bar or background tint, and must remain legible in both light and dark mode.

Weekly activity chart placeholder — Below the cards, a full-width div with a dashed border and a centered label (e.g., "Weekly Activity Chart") stands in for a future chart. It is non-interactive and scales to the container width.

2. User Management

Users table — A table listing all registered users with columns: Name, Email, Plan, and Status. Status renders as a colored badge (e.g., active = green, suspended = red). Rows are populated from hardcoded mock data and the table is horizontally scrollable on narrow viewports.

Row action dropdown — Each row ends with a ⋮ button that toggles a small dropdown menu containing at least "View detail" and "Delete". Only one dropdown is open at a time; clicking elsewhere closes it. "Delete" removes the row from the table in-session.

User detail modal — Choosing "View detail" opens a modal overlay showing the full user record. The modal closes via an explicit close button and by clicking the backdrop. The backdrop dims the page; the modal is centered and traps focus visually.

3. Agent Management

Agents list — A listing of all agents showing agent name, owner, current status (active / inactive / failing), and a collapsed skill list. Status is a color-coded badge. Each agent renders as a row or card from hardcoded data.

Expandable skill list — Each agent's skills are hidden by default behind an expand control (e.g., a chevron). Clicking it reveals the full skill list with a smooth height/opacity transition and toggles the chevron direction. Collapsing animates back. Each agent expands independently.

Configure / Delete dropdown — Each agent has a ⋮ action dropdown with "Configure" and "Delete". "Configure" opens a modal displaying the agent's system prompt (read-only text block). "Delete" removes the agent in-session. The modal closes via button and backdrop click.

4. Skills

Skill catalog — A grid or list of skill entries, each showing a name, a short description, and an indicator of how many agents currently have it enabled (e.g., a count badge). Entries come from hardcoded data and lay out responsively (multi-column on desktop, single column on mobile).

"What is a skill?" explainer — A brief in-panel info block (callout or banner) explaining that a skill is a modular capability — such as web browsing, document reading, or calendar management — that can be attached to an agent. Visually distinct from the catalog (tinted background or border).

Skill action dropdown — Each skill has a ⋮ dropdown with "View detail" and "Delete". "View detail" opens a modal with the skill's full description and usage count; "Delete" removes it in-session. Standard single-open dropdown and backdrop-dismiss modal behavior applies.

5. Agent Contracts

Contracts table — A table of all active and past rental contracts with columns: Client, Rented Agent, Contracted Skills, Contract Dates (start–end), and Total Amount Paid. Contracted Skills may render as a compact list or chips. Data is hardcoded; the table scrolls horizontally on small screens.

Row action dropdown — Each row has a ⋮ dropdown including "View detail". Single-open behavior; closes on outside click.

Contract detail modal — "View detail" opens a modal showing the full contract breakdown: client and agent info, contract dates, total paid, and an itemized list of contracted skills with each skill's individual price. Closes via button and backdrop click.

6. Error Log

Error log table — A log listing execution errors with columns: Timestamp, Agent Name, Error Type, and a short Description. Populated from hardcoded data, ordered newest-first.

Color-coded severity badges — Each entry's error type/severity renders as a color-coded badge (e.g., critical = red, warning = amber, info = blue/gray), legible in both themes, so the admin can scan severity at a glance.

View detail / Mark as resolved dropdown — Each entry has a ⋮ dropdown with "View detail" (opens a modal showing the full error trace in a monospaced block) and "Mark as resolved" (visually marks or removes the entry in-session, e.g., dimming the row or updating its status). Modal closes via button and backdrop.

Component Inventory

These are the reusable UI components shared across multiple sections. Each is defined once here; section specs reference these rather than redefining them.

Sidebar — A persistent vertical navigation fixed to the left edge, present on every view. Contains the AgentHub logo/title and a link for each of the six sections, each with an icon and label. The active section is visually highlighted. The sidebar stays fixed while content scrolls and adapts in both light and dark mode. On narrow viewports it may collapse to icons or a toggleable drawer.

Metric card — A rectangular card containing an icon, a text label, and a hardcoded value. Carries a subtle shadow, rounded corners, and a metric-specific accent color. Used on the Dashboard; built to be repeatable in a grid. Non-interactive aside from a slight hover elevation.

Action dropdown — A ⋮ (vertical ellipsis) button that toggles a small floating menu of actions. Shared by every table/list row (Users, Agents, Skills, Contracts, Errors). Global rules: only one dropdown is open at any time, opening one closes others, and clicking anywhere outside closes the open menu. Menu options vary by section but the trigger and open/close behavior are identical everywhere.

Modal — A centered overlay dialog over a dimmed backdrop, used for all detail and configuration views (user detail, agent configure, skill detail, contract detail, error trace). Global rules: closes via an explicit close button (×) and by clicking the backdrop; clicking inside the modal body does not close it; only one modal is open at a time. Content is injected per use case; the shell and dismiss behavior are shared.

Badge — A small rounded pill displaying a status or category with a background color mapped to its meaning. Used for user status, agent status, and error severity. Color mapping is consistent platform-wide (e.g., green = active/healthy, amber = warning, red = failing/critical, neutral = inactive/info) and stays legible in both themes.

Collapsible skill list — A list of skills hidden by default behind an expand control (chevron). Toggling animates the list open/closed with a smooth height/opacity transition and flips the chevron. Primarily used in Agent Management; each instance expands independently of others.

Dark mode toggle — A control in the top bar that switches the entire interface between light and dark mode by toggling the dark class on the root element, driving Tailwind's dark: utilities. Every component above must render correctly in both modes. (Persistence is not required for the prototype; default theme on load is acceptable.)

Acceptance Criteria
The prototype is considered complete when all of the following are true:
Navigation & layout

1. The sidebar is visible on every section and clicking each of the six links displays the corresponding section.

2. The active section is visually highlighted in the sidebar.

Dropdown

3. Clicking a ⋮ button opens that row's action menu.

4. Opening one dropdown closes any other open dropdown (only one open at a time).

5. Clicking anywhere outside an open dropdown closes it.

Modal

6. Choosing a detail/configure action ("View detail", "Configure") opens the correct modal populated with that item's data.

7. The modal closes when the close (×) button is clicked.

8. The modal closes when the backdrop is clicked, and does not close when the modal body itself is clicked.

Collapsible skill list

9. An agent's skill list is collapsed by default.

10. Clicking the expand control reveals that agent's skills with a smooth transition and flips the chevron, and clicking again collapses it — each agent toggling independently.

Dark mode

11. Toggling the top-bar control switches the entire interface between light and dark mode.

12. All sections and components — sidebar, cards, tables, badges, dropdowns, modals — render legibly in both modes.

Section content

13. The Dashboard shows four metric cards (with icons, labels, hardcoded values, distinct accents) and a dashed-border chart placeholder below.

14. Users, Agents, Skills, Contracts, and Errors each render their data from mock data with the columns/fields defined in their section specs.

15. Status and severity badges are color-coded consistently per the badge color mapping.

16. Section-specific actions behave as specified: Delete removes the item in-session; "Mark as resolved" updates the error entry; the Skills section includes the "what is a skill?" explainer; the contract detail modal lists itemized skills with individual prices.
