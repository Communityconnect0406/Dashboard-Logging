










<html lang="en">
<head>
<meta charset="UTF-8">
<title>Punishment Logging Console</title>
<style>
    :root {
        --bg: #05060a;
        --card: rgba(15, 17, 25, 0.9);
        --accent: #5865F2;
        --accent-soft: rgba(88, 101, 242, 0.25);
        --text-main: #ffffff;
        --text-soft: #a9b1c6;
        --border-subtle: rgba(255,255,255,0.06);
        --good: #3ba55d;
        --warn: #faa81a;
        --danger: #ed4245;
        --muted: #3c82e3;
        --note: #80848e;
        --blacklist: #111111;
    }
    * { box-sizing: border-box; }
    body {
        margin: 0;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background: radial-gradient(circle at top, #1a1b26 0, #05060a 55%);
        font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
        color: var(--text-main);
    }
    .shell {
        width: 100%;
        max-width: 1300px;
        padding: 20px;
    }
    .title {
        font-size: 22px;
        font-weight: 700;
        margin-bottom: 6px;
        letter-spacing: 0.03em;
    }
    .subtitle {
        font-size: 13px;
        color: var(--text-soft);
        margin-bottom: 18px;
    }
    .layout {
        display: grid;
        grid-template-columns: 1.1fr 0.9fr;
        gap: 18px;
        margin-bottom: 16px;
    }
    .card {
        background: var(--card);
        border-radius: 16px;
        padding: 18px 18px 16px;
        border: 1px solid var(--border-subtle);
        box-shadow: 0 18px 40px rgba(0,0,0,0.55);
        backdrop-filter: blur(18px);
    }
    .card-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 10px;
    }
    .card-title {
        font-size: 14px;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.08em;
        color: var(--text-soft);
    }
    .pill {
        font-size: 11px;
        padding: 3px 8px;
        border-radius: 999px;
        background: var(--accent-soft);
        color: #cdd4ff;
    }
    label {
        display: block;
        font-size: 12px;
        margin-top: 10px;
        margin-bottom: 4px;
        color: var(--text-soft);
    }
    input, textarea, select {
        width: 100%;
        padding: 9px 10px;
        border-radius: 10px;
        border: 1px solid var(--border-subtle);
        background: rgba(10, 11, 18, 0.9);
        color: var(--text-main);
        font-size: 13px;
        outline: none;
        transition: border 0.15s, box-shadow 0.15s, background 0.15s;
    }
    input:focus, textarea:focus, select:focus {
        border-color: var(--accent);
        box-shadow: 0 0 0 1px rgba(88,101,242,0.6);
        background: rgba(10, 11, 18, 1);
    }
    textarea {
        resize: vertical;
        min-height: 80px;
        max-height: 200px;
    }
    .row {
        display: flex;
        gap: 10px;
    }
    .row > div { flex: 1; }

    .toggle-row {
        display: flex;
        align-items: center;
        gap: 8px;
        font-size: 12px;
        color: var(--text-soft);
        margin-top: 4px;
    }
    .toggle-row input[type="checkbox"] {
        width: auto;
    }

    button {
        margin-top: 14px;
        width: 100%;
        padding: 10px;
        border-radius: 999px;
        border: none;
        background: linear-gradient(135deg, #5865F2, #3c45b5);
        color: white;
        font-weight: 600;
        font-size: 13px;
        cursor: pointer;
        letter-spacing: 0.04em;
        text-transform: uppercase;
        box-shadow: 0 10px 25px rgba(88,101,242,0.55);
        transition: transform 0.08s ease-out, box-shadow 0.08s ease-out, filter 0.08s;
    }
    button:hover {
        transform: translateY(-1px);
        box-shadow: 0 14px 32px rgba(88,101,242,0.7);
        filter: brightness(1.05);
    }
    button:active {
        transform: translateY(0);
        box-shadow: 0 8px 18px rgba(88,101,242,0.5);
        filter: brightness(0.98);
    }

    .preview-title {
        font-size: 15px;
        font-weight: 600;
        margin-top: 4px;
    }
    .preview-sub {
        font-size: 12px;
        color: var(--text-soft);
        margin-top: 4px;
    }
    .preview-block {
        margin-top: 8px;
        font-size: 12px;
        padding: 8px 10px;
        border-radius: 10px;
        background: rgba(255,255,255,0.03);
        border: 1px solid rgba(255,255,255,0.05);
    }

    .status-line {
        margin-top: 10px;
        font-size: 11px;
        color: var(--text-soft);
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .status-left {
        display: flex;
        align-items: center;
        gap: 4px;
    }
    .status-dot {
        width: 8px;
        height: 8px;
        border-radius: 999px;
        background: var(--good);
    }

    .case-id {
        font-size: 11px;
        color: var(--text-soft);
        margin-top: 6px;
    }

    .history-layout {
        display: grid;
        grid-template-columns: 2fr 3fr;
        gap: 12px;
        margin-top: 10px;
    }
    .history-list {
        max-height: 260px;
        overflow-y: auto;
        font-size: 11px;
    }
    .history-item {
        padding: 6px 8px;
        border-radius: 8px;
        border: 1px solid rgba(255,255,255,0.04);
        margin-bottom: 6px;
        cursor: pointer;
        transition: background 0.12s, border 0.12s;
    }
    .history-item:hover {
        background: rgba(255,255,255,0.03);
        border-color: rgba(255,255,255,0.08);
    }
    .history-item-main {
        display: flex;
        justify-content: space-between;
        margin-bottom: 2px;
    }
    .history-item-user {
        font-weight: 600;
        font-size: 11px;
    }
    .history-item-punish {
        font-size: 11px;
    }
    .history-item-meta {
        font-size: 10px;
        color: var(--text-soft);
    }

    .history-detail {
        font-size: 11px;
        color: var(--text-soft);
        max-height: 260px;
        overflow-y: auto;
    }
    .history-detail h4 {
        margin: 0 0 4px;
        font-size: 12px;
        color: var(--text-main);
    }
    .history-detail p {
        margin: 2px 0;
    }

    .history-controls {
        display: flex;
        gap: 6px;
        margin-top: 6px;
    }
    .history-controls button {
        margin-top: 0;
        padding: 6px 8px;
        font-size: 11px;
        box-shadow: none;
    }

    .search-row {
        display: flex;
        gap: 6px;
        margin-bottom: 6px;
    }
    .search-row input, .search-row select {
        font-size: 11px;
        padding: 6px 8px;
    }

    @media (max-width: 1050px) {
        .layout {
            grid-template-columns: 1fr;
        }
        .history-layout {
            grid-template-columns: 1fr;
        }
    }
</style>
</head>
<body>
<div class="shell">
    <div class="title">Punishment Logging Console</div>
    <div class="subtitle">
        Log punishments with pings, expiration, appeal status, case fingerprints, webhook delivery, and local history.
    </div>

    <div class="layout">
        <!-- FORM PANEL -->
        <div class="card">
            <div class="card-header">
                <div class="card-title">New punishment</div>
                <div class="pill">Logging · Webhook + Local History</div>
            </div>

            <div class="row">
                <div>
                    <label>Discord Username</label>
                    <input id="username" placeholder="e.g. AVG#0001">
                </div>
                <div>
                    <label>Discord ID</label>
                    <input id="userid" placeholder="e.g. 123456789012345678">
                </div>
            </div>

            <div class="toggle-row">
                <input type="checkbox" id="pingUser">
                <span>Ping user in log (<@ID>)</span>
            </div>

            <label>Staff member (optional)</label>
            <input id="staff" placeholder="e.g. ModName#0001">

            <label>Punishment type</label>
            <select id="punishment">
                <option>Warning</option>
                <option>Verbal Warning</option>
                <option>Kick</option>
                <option>Mute</option>
                <option>Temp Mute</option>
                <option>Temp Ban</option>
                <option>Server Ban</option>
                <option>Global Ban</option>
                <option>Blacklist</option>
                <option>Note</option>
            </select>

            <label>Reason (short)</label>
            <input id="reason" placeholder="Short reason, e.g. 'Harassment in chat'">

            <label>Description (full details)</label>
            <textarea id="description" placeholder="Full context, logs, and any relevant notes..."></textarea>

            <label>Appeal allowed?</label>
            <select id="appeal">
                <option value="yes">Yes – user can appeal</option>
                <option value="no">No – appeal not allowed</option>
            </select>

            <label>Expiration (duration)</label>
            <select id="duration">
                <option value="none">Select duration (or use manual below)</option>
                <option value="1h">1 hour</option>
                <option value="12h">12 hours</option>
                <option value="1d">1 day</option>
                <option value="3d">3 days</option>
                <option value="7d">7 days</option>
                <option value="30d">30 days</option>
                <option value="perm">Permanent</option>
            </select>

            <label>Expiration (manual override)</label>
            <input id="expiresManual" placeholder="YYYY-MM-DD HH:MM (24h, server time)">

            <button id="logBtn">Log punishment & send to webhook</button>

            <div class="status-line">
                <div class="status-left">
                    <div class="status-dot" id="statusDot"></div>
                    <span id="statusText">Ready to log.</span>
                </div>
                <span id="webhookStatus">Webhook: queued on log</span>
            </div>

            <div class="case-id" id="caseIdBox">
                Case fingerprint: –
            </div>
        </div>

        <!-- PREVIEW PANEL -->
        <div class="card">
            <div class="card-header">
                <div class="card-title">Preview</div>
                <div>
                    <span class="badge">
                        <span class="badge-dot"></span>
                        Discord-style embed
                    </span>
                    <span class="badge">
                        Local history enabled
                    </span>
                </div>
            </div>

            <div class="preview-title" id="previewTitle">Punishment: –</div>
            <div class="preview-sub" id="previewUser">User: –</div>

            <div class="preview-block" id="previewCore">
                <strong>Core info</strong><br>
                Punishment: –<br>
                Reason: –<br>
                Appeal: –<br>
                Ping: –<br>
            </div>

            <div class="preview-block" id="previewExpiry">
                <strong>Expiration</strong><br>
                Expires at: –<br>
                Expires in: –<br>
            </div>

            <div class="preview-block" id="previewMeta">
                <strong>Metadata</strong><br>
                Case fingerprint: –<br>
                Timestamp: –<br>
                Staff: –<br>
            </div>

            <div class="preview-block" id="previewDescription">
                <strong>Description</strong><br>
                –
            </div>
        </div>
    </div>

    <!-- HISTORY PANEL -->
    <div class="card">
        <div class="card-header">
            <div class="card-title">Local case history</div>
            <div class="pill">Stored in browser · Not shared</div>
        </div>

        <div class="history-layout">
            <div>
                <div class="search-row">
                    <input id="searchInput" placeholder="Search by user, ID, case ID, reason...">
                    <select id="filterPunishment">
                        <option value="">All punishments</option>
                        <option>Warning</option>
                        <option>Verbal Warning</option>
                        <option>Kick</option>
                        <option>Mute</option>
                        <option>Temp Mute</option>
                        <option>Temp Ban</option>
                        <option>Server Ban</option>
                        <option>Global Ban</option>
                        <option>Blacklist</option>
                        <option>Note</option>
                    </select>
                    <select id="filterAppeal">
                        <option value="">Appeal: Any</option>
                        <option value="yes">Appeal allowed</option>
                        <option value="no">Appeal not allowed</option>
                    </select>
                    <select id="filterExpired">
                        <option value="">Status: Any</option>
                        <option value="active">Active</option>
                        <option value="expired">Expired</option>
                    </select>
                </div>

                <div class="history-list" id="historyList">
                    <!-- items -->
                </div>

                <div class="history-controls">
                    <button id="exportBtn">Export JSON</button>
                    <button id="clearBtn" style="background:#ed4245;">Clear history</button>
                </div>
            </div>

            <div class="history-detail" id="historyDetail">
                <h4>No case selected</h4>
                <p>Select a case from the list to view full details.</p>
            </div>
        </div>
    </div>
</div>

<script>
/* NEW WEBHOOK URL */
const WEBHOOK_URL = "https://discord.com/api/webhooks/1488343805550792878/aSGIwUxsescqctrzgpvbTp2XOM_HBxkOkr38gxaesHCMtqgJkMlNUaTLQPZRZ9ZK0FRu";

/* COLOR MAP */
function punishmentColor(p) {
    switch (p) {
        case "Warning":
        case "Verbal Warning": return "#faa81a";
        case "Kick": return "#f57731";
        case "Mute":
        case "Temp Mute": return "#3c82e3";
        case "Temp Ban": return "#9b59b6";
        case "Server Ban": return "#ed4245";
        case "Global Ban": return "#8b0000";
        case "Blacklist": return "#111111";
        case "Note": return "#80848e";
        default: return "#5865F2";
    }
}

/* CASE ID */
function generateCaseFingerprint(username, userId, punishment, timestamp) {
    const base = (username + "|" + userId + "|" + punishment + "|" + timestamp).trim();
    let hash = 0;
    for (let i = 0; i < base.length; i++) {
        hash = ((hash << 5) - hash) + base.charCodeAt(i);
        hash |= 0;
    }
    return "CASE-" + Math.abs(hash).toString(16).toUpperCase().slice(0, 8);
}

/* DATE HELPERS */
function parseManualDate(str) {
    if (!str) return null;
    const parts = str.trim().split(" ");
    if (parts.length !== 2) return null;
    const [datePart, timePart] = parts;
    const [y, m, d] = datePart.split("-").map(Number);
    const [hh, mm] = time
