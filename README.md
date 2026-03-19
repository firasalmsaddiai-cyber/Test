# Test
Build me a single-file index.html dashboard called "FAM Properties — CEO War Room Dashboard" for March 2026. This is a dark-themed, data-dense CEO operations dashboard for a real estate brokerage. It must be a single HTML file with all CSS and JS inline — no frameworks, no build tools, no external dependencies except these 3 CDN scripts in <head>:

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
Also create a server.rb file that serves the dashboard locally on port 3333 using WEBrick.

DATA FORMAT

The broker data must be embedded directly in a <script> tag as a var RAW = { agents: [...], streaks: [...] } object, using compact short keys to minimize file size:

agents array — each object:

id (agent_id), n (agent_name), m (month), h (hire_date), t (team)
wd (total_working_days), ad (attended_days), lc (lead_mine_claims)
rp (reveal_prospects_count), lp (lead_mine_page_access)
cd (closed_deals), pl (primary_listing), sl (secondary_listing)
streaks array — each object:

id (agent_id), s (streak months without a deal), ld (last_deal date or "NEVER"), lt (lifetime total deals)
Then map RAW into full-named agents array for use throughout the dashboard.

IMPORTANT: Read whatever CSV/JSON data files exist in my working directory and embed ALL agent data directly into the HTML file. Ask me for the data files if none are found.

DESIGN SYSTEM

Dark theme with these exact CSS variables:

--bg: #0a0e17
--card: #111827
--border: #1e293b
--accent: #f59e0b (amber)
--red: #ef4444
--green: #22c55e
--blue: #3b82f6
--orange: #f97316
--text: #e2e8f0
--muted: #64748b
--danger-bg: #1c1017
--danger-border: #7f1d1d
--purple: #a855f7
Font: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif Max container width: 1600px. Border-radius: 12px on cards, 8px on inputs/buttons.

HEADER

Sticky header with:

Left: "FAM Properties — CEO War Room" (CEO War Room in red)
Right: green pulsing live dot + "March 2026", agent count, PDF Report button (red gradient), Excel button (green gradient), Ask Claude button (amber gradient)
AGENT CLASSIFICATION ENGINE

Classify every agent into one of these statuses based on a scoring algorithm:

Activity Score = weighted sum of: primary_listing × 3 + secondary_listing × 2 + lead_mine_claims × 4 + reveal_prospects_count × 1 + lead_mine_page_access × 0.5

Classification rules (applied in order):

super_active: closed_deals > 0 AND score > 80
active: closed_deals > 0
severe: closed_deals === 0 AND score >= 130 AND streak >= 2 months (subdivide into ghost = low activity score, parasite = high activity + zero closings)
suspicious: closed_deals === 0 AND score >= 50 AND score < 130
watch: closed_deals === 0 AND score >= 20 AND score < 50
clear: everything else
Enrich each classified agent with their streak data (sk property containing streak, lastDeal, lifetime).

Note in the dashboard that 145 separated agents have been removed from the dataset (show this as a subtle note).

KPI ROW

Top row of KPI cards (auto-fit grid, min 170px):

Active Brokers (total count)
Deals Closed (sum of closed_deals)
Closers (agents with ≥1 deal, show count + % closing rate)
Severely Suspicious (count, red danger card style)
Suspicious (count, orange warning card style)
Never Closed (count of flagged agents who NEVER closed in 15 months, red danger card)
Watch List (count)
Super Active (count, gold gradient card with star)
Avg Attendance %
Total Listings (primary + secondary combined)
Lead Claims (total)
Reveals (total)
EXECUTIVE SUMMARY BOX

A text summary block below KPIs with color-highlighted key stats (red for bad, green for good, orange for warnings). Describe the brokerage health in 3-4 sentences using the computed metrics.

SECTION 1: FLAGGED AGENTS (Tabbed)

Tab bar with 3 tabs: Severe (default, red badge), Suspicious (orange badge), Watch (purple badge).

Table columns: Risk Level (with SEVERE/SUS/WATCH badge + ghost or parasite subtype for severe), Score, Agent Name (clickable link), Team, Streak Badge, Last Deal, Lifetime Deals, Attendance (days/total), Primary Listings, Secondary Listings, Lead Claims, Reveals, Red Flags (auto-generated text like "NEVER closed", "6mo dry", "Heavy lister", "Lead hoarder", "Mass reveals").

Streak badges: color-coded spans — streak-crit (>=6mo, red), streak-warn (3-5mo, orange), streak-ok (<3mo, green), streak-never (NEVER, bright red bold).

Row highlighting: ghost-row for severe (subtle red bg), sus-row for suspicious (subtle orange bg).

SECTION 2: INDUSTRY BENCHMARKS

A benchmark comparison box showing how FAM's metrics compare to industry standards. Include items like:

Closing rate vs industry average
Activity-to-deal conversion
Attendance patterns
Listing quality flags
Each benchmark item in a card with icon, title, description, and color-coded severity (severe/sus border styles). Use a responsive grid (min 320px columns).

SECTION 3: TEAM PERFORMANCE

Grid of team cards (min 270px). Each card shows:

Team name + agent count
Deals closed (green if >0, red if 0)
Closers ratio (X/Y)
Severe/Suspicious count
Conversion % (deals/listings)
Health bar (colored: green >60%, orange 30-60%, red <30%)
Cards are clickable — clicking opens a Team Detail Panel (slide-in from left, 560px wide) with:

Team KPIs (6-column grid: agents, deals, closers, conversion, severe/sus, never closed)
PDF download button + Excel download button for that team
Full agent table for the team sorted by status severity then score
SECTION 4: TOP CLOSERS

Table showing top 15 agents by closed_deals. Columns: Rank (#), Agent (clickable), Team, Deals (green), Lifetime, Listings, Reveals, Attendance %, Tenure.

SECTION 5: FULL ROSTER

Searchable, sortable table of ALL agents.

Search input above table (filters rows by text match)
Sortable column headers (click to toggle asc/desc, show arrow indicators in amber)
Columns: Agent (clickable), Team, Status (colored tag), Score, Streak Badge, Last Deal, Lifetime, Attendance (days/total), Deals, Listings, Claims, Reveals
Scrollable (max-height 600px, custom scrollbar)
Row highlighting for severe/suspicious
AGENT DETAIL PANEL

Right-side slide-in panel (480px wide) that opens when clicking any agent name. Shows:

Agent name, team, tenure, hire date
Status tag + score + streak badge
2-column stat grid (10 stats): Deals (March), Lifetime Deals, Last Deal, Dry Streak (months), Attendance (with %), Primary Listings, Secondary Listings, Lead Claims, Reveals, Page Access
"Discuss with Claude" button (amber gradient) — copies a pre-built analysis prompt to clipboard and opens claude.ai
CLAUDE CHAT PANEL

Floating action button (FAB) in bottom-right corner (56px circle, amber gradient, chat icon). Clicking it opens a chat panel (420x600px, dark card) with:

Header: "Claude x War Room" with close button

Welcome state shows 4 action buttons:

Generate CEO Report — full analysis with cut/keep/action items
Worst Performing Teams — bottom 5 teams by metrics
Generate Cut List — agents to terminate
Agents to Protect — top producers needing retention
How it works: Each action builds a detailed prompt containing the full dashboard context (all agent data, team summaries, flagged lists) and:

Copies the prompt to clipboard
Shows the prompt preview in the panel
Provides an "Open Claude.ai" button that opens a new tab
Provides a "Copy Again" button
Shows instructions to paste (Cmd+V / Ctrl+V)
Free-form chat input at the bottom — user types any question, it builds context + question prompt and does the same clipboard flow.

Context builder function (buildContext) must include: total agents, teams count, deals, closing rate, severe/suspicious counts, never-closed count, avg attendance, top 10 severe agents with full stats, top 10 closers, and top 15 team summaries.

Agent-specific context (buildAgentContext) includes all individual agent metrics for deep-dive analysis.

PDF REPORT GENERATION (jsPDF)

Clicking "PDF Report" in the header generates a multi-page PDF:

Page 1 — Cover:

Dark navy background (#0f172a), red accent bar at top (3mm)
Title: "CEO War Room Report" (28pt white)
Subtitle: "FAM Properties — March 2026" (14pt amber)
"CONFIDENTIAL" + generation date
Executive summary box with 6 bullet points
6 KPI boxes in 3x2 grid (brokers, deals, severe, suspicious, never closed, closing rate)
Page 2 — Severely Suspicious Agents:

Heading in red, subtitle with count
Auto-table: Score, Agent, Team, Last Deal, Dry Streak, Lifetime, Listings, Claims, Reveals, Attendance
"NEVER" cells highlighted in red bold
Dark theme styling (navy headers with amber text, dark card body, alternating rows)
Page 3 — Suspicious Agents:

Heading in orange, same table format
Page 4 — Team Performance:

Blue heading, table: Team, Agents, Deals, Closers, Listings, Claims, Conv%, Severe, Sus, Attendance
Zero-deal teams in red, flagged counts color-coded
Page 5 — Top 15 Closers:

Heading in green
Rank, Agent, Team, Deals (green), Lifetime, Listings, Claims, Reveals, Attendance, Tenure
Page 6 — Wartime Recommendations:

4 recommendation boxes with colored left borders:
IMMEDIATE ACTION (red) — termination targets
WITHIN 2 WEEKS (orange) — manager interventions
PROTECT & RETAIN (green) — revenue backbone
PLATFORM & PROCESS (blue) — operational improvements
Every page: header bar with "FAM Properties — CEO War Room Report — March 2026 | CONFIDENTIAL", page numbers "Page X of Y".

TEAM PDF GENERATION

Each team panel has its own PDF download. Landscape A4 with:

Dark cover header with team name, date, confidential
KPI row (7 boxes: agents, deals, closers, conversion, severe, suspicious, never closed)
Full agent table with all columns, status color-coded
Page numbers and confidential footer
EXCEL DOWNLOADS

Full Excel (header button) generates a multi-sheet workbook:

All Agents — full roster sorted by status severity then score
Team Summary — one row per team with aggregated metrics
Flagged Agents — severe + suspicious combined
One sheet per team — individual team rosters
Each sheet has proper column widths. Columns: Agent Name, Team, Status, Activity Score, Deals (March), Lifetime Deals, Last Deal, Dry Streak (mo), Attendance %, Attended Days, Working Days, Primary Listings, Secondary Listings, Lead Claims, Reveals, Page Access, Hire Date.

Team Excel (team panel button) generates single-sheet workbook for that team only.

INTERACTIVE FEATURES

Column sorting on all tables — click header to toggle asc/desc, show amber arrow indicator
Search/filter on Full Roster — real-time text filtering
Tab switching on Flagged Agents section
Clickable agent names throughout — open agent detail panel
Clickable team cards — open team detail panel
Scrollable tables with custom dark scrollbar (6px, rounded)
HELPER FUNCTIONS NEEDED

tenureLabel(hire_date) — returns human-readable tenure like "2y 3m", "8m", etc.
getStreak(agent_id) — looks up streak data, returns {streak, lastDeal, lifetime}
streakBadge(sk) — returns HTML span with color-coded streak
agentLink(name, id) — returns clickable <a> with class agent-link and data-aid attribute
z(val) — returns value or <span class="zero">0</span> for zero values
CRITICAL REQUIREMENTS

Everything in ONE index.html file — CSS in <style>, JS in <script>, data embedded
No server-side rendering — pure client-side
No public access — this is a local-only tool served via the Ruby WEBrick server on localhost:3333
No authentication or external API calls — all data is embedded, Claude integration is clipboard-based only
Mobile-responsive where possible (grid breakpoints at 900px)
Performant — should handle 500+ agents without lag
All agent data must be embedded from the data files in the working directory  
