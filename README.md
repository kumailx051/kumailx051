import { useState } from "react";

const MARKDOWN_TEMPLATE = `<!-- GitHub Profile README — Neofetch Style
     ─────────────────────────────────────────────────────
     HOW TO ACTIVATE: Create a PUBLIC repository named
     EXACTLY your GitHub username. e.g. if you're @kumail,
     create a repo called "kumail". GitHub will automatically
     display its README.md on your profile page.
     ───────────────────────────────────────────────────── -->

<div align="center">

\`\`\`
                                       yourusername@github
   [PASTE YOUR ASCII ART HERE]         ──────────────────────────────────────────
                                       OS: Kali Linux / Windows 11
   Steps to generate ASCII art:        Uptime: 24 years, 2 months, 15 days
   1. manytools.org                    Host: Bahria University, Islamabad
   2. Hacker Tools section             Kernel: Cybersecurity & IoT Security
   3. "Convert image to ASCII art"     IDE: VS Code 1.99.0, PyCharm
   4. Upload your photo                
   5. Width: ~22 characters            Languages.Programming: Python, C/C++, Java
   6. Copy output and paste here       Languages.Computer: HTML, CSS, SQL, Bash
                                       Languages.Real:        Urdu, English
                                       
                                       Hobbies.Cyber: CTFs, Penetration Testing
                                       Hobbies.Dev:   IoT Security, Open Source
                                       
                                       Contact:
                                         Email:    you@email.com
                                         LinkedIn: linkedin.com/in/yourusername
                                         GitHub:   github.com/yourusername
                                       
                                       GitHub Stats
                                       ─────────────────────────────────
                                       ★ Stars:          42
                                       Commits (2025):   183
                                       Followers:        29
\`\`\`

</div>

<!-- Dynamic stats cards — replace YOUR_USERNAME with your actual GitHub handle -->

[![GitHub Stats](https://github-readme-stats.vercel.app/api?username=YOUR_USERNAME&show_icons=true&theme=radical&hide_border=true)](https://github.com/YOUR_USERNAME)

[![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_USERNAME&layout=compact&theme=radical&hide_border=true)](https://github.com/YOUR_USERNAME)

<!-- Skill badges — add yours from shields.io -->
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Kali](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)
`;

const BADGE_SNIPPETS = [
  { label: "Python", code: `![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)` },
  { label: "C++", code: `![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)` },
  { label: "Kali Linux", code: `![Kali](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)` },
  { label: "VS Code", code: `![VS Code](https://img.shields.io/badge/VS_Code-007ACC?style=for-the-badge&logo=visual-studio-code&logoColor=white)` },
  { label: "GitHub Actions", code: `![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)` },
];

const steps = [
  {
    emoji: "🏗️",
    title: "Create the Profile Repository",
    items: [
      'Go to GitHub → click "+" → "New repository"',
      "Name it EXACTLY your GitHub username (e.g. if you're @johndoe, name it johndoe)",
      "Set it to Public",
      'Check "Add a README file" → Create repository',
      "GitHub will now show this repo's README on your profile page"
    ]
  },
  {
    emoji: "🖼️",
    title: "Generate ASCII Art from Your Photo",
    items: [
      "Go to manytools.org → Hacker Tools",
      '"Convert image to ASCII art" → Upload your photo',
      "Set character width to 20–24 characters",
      "Choose block characters (█ ▓ ░) for a clean look",
      "Copy the output — this becomes the left column"
    ]
  },
  {
    emoji: "✏️",
    title: "Edit README.md",
    items: [
      "In your new repo, click the pencil (edit) icon on README.md",
      "Delete everything and paste the template from the Template tab",
      "Replace [PASTE YOUR ASCII ART HERE] with your generated art",
      "Fill in your real info (OS, university, skills, contact)",
      "Replace YOUR_USERNAME with your GitHub handle in the stats URLs"
    ]
  },
  {
    emoji: "📐",
    title: "Align the Two Columns",
    items: [
      "The code block renders in monospace — spacing controls alignment",
      "Each left-column line should be the same total width",
      "Use spaces to pad shorter left-column lines so the right column stays straight",
      "Preview in GitHub's editor before committing to check alignment"
    ]
  },
  {
    emoji: "🚀",
    title: "Commit and View",
    items: [
      'Click "Commit changes"',
      "Visit github.com/YOUR_USERNAME",
      "Your neofetch profile is now live! 🎉",
      "Keep tweaking — spacing and content are easy to update"
    ]
  }
];

const tools = [
  { name: "manytools.org", section: "Hacker Tools → Convert image to ASCII", desc: "Best tool for photo-to-ASCII, block character style", tag: "ASCII Art" },
  { name: "asciiart.club", section: "", desc: "Alternative image-to-ASCII converter", tag: "ASCII Art" },
  { name: "github-readme-stats", section: "github.com/anuraghazra/github-readme-stats", desc: "Dynamic stats, language, and streak cards", tag: "Stats" },
  { name: "shields.io", section: "", desc: "Custom skill, language, and social badges", tag: "Badges" },
  { name: "readme.so", section: "", desc: "Visual drag-and-drop README builder", tag: "Builder" },
  { name: "github.com/abhisheknaiidu/awesome-github-profile-readme", section: "", desc: "Gallery of great profile READMEs for inspiration", tag: "Inspo" },
];

const TAG_COLORS = {
  "ASCII Art": "#1f6feb",
  "Stats": "#388bfd",
  "Badges": "#7ee787",
  "Builder": "#ffa657",
  "Inspo": "#bc8cff",
};

export default function App() {
  const [tab, setTab] = useState("preview");
  const [copied, setCopied] = useState(false);
  const [copiedBadge, setCopiedBadge] = useState(null);

  const C = {
    bg: "#0d1117",
    card: "#161b22",
    el: "#21262d",
    border: "#30363d",
    blue: "#58a6ff",
    blueBright: "#79c0ff",
    green: "#7ee787",
    orange: "#ffa657",
    muted: "#8b949e",
    text: "#e6edf3",
    textDim: "#c9d1d9",
  };

  const copyTemplate = () => {
    navigator.clipboard.writeText(MARKDOWN_TEMPLATE);
    setCopied(true);
    setTimeout(() => setCopied(false), 2200);
  };

  const copyBadge = (code, label) => {
    navigator.clipboard.writeText(code);
    setCopiedBadge(label);
    setTimeout(() => setCopiedBadge(null), 1800);
  };

  const tabBtn = (id, label) => (
    <button
      key={id}
      onClick={() => setTab(id)}
      style={{
        padding: "7px 14px",
        background: tab === id ? C.blue : "transparent",
        color: tab === id ? C.bg : C.muted,
        border: "none",
        borderRadius: "6px",
        cursor: "pointer",
        fontSize: "12px",
        fontWeight: tab === id ? "700" : "400",
        transition: "all 0.15s",
        whiteSpace: "nowrap"
      }}
    >
      {label}
    </button>
  );

  return (
    <div style={{ background: C.bg, minHeight: "100vh", color: C.text, padding: "18px 16px", fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif", boxSizing: "border-box" }}>
      <div style={{ maxWidth: "760px", margin: "0 auto" }}>

        {/* Header */}
        <div style={{ marginBottom: "18px" }}>
          <h1 style={{ color: C.blue, fontSize: "17px", fontWeight: 700, margin: "0 0 4px 0", letterSpacing: "-0.3px" }}>
            ⚡ Neofetch-Style GitHub Profile README
          </h1>
          <p style={{ color: C.muted, fontSize: "12px", margin: 0 }}>
            The terminal-aesthetic profile with ASCII art + system info — built from plain Markdown
          </p>
        </div>

        {/* Tabs */}
        <div style={{ background: C.el, borderRadius: "8px", padding: "3px", display: "flex", gap: "1px", width: "fit-content", marginBottom: "16px", flexWrap: "wrap" }}>
          {[["preview","👁 Preview"], ["steps","📋 Steps"], ["template","📄 Template"], ["tools","🔗 Tools"]].map(([id, label]) => tabBtn(id, label))}
        </div>

        {/* ── PREVIEW ── */}
        {tab === "preview" && (
          <div>
            {/* Terminal window */}
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: "10px", overflow: "hidden", boxShadow: "0 8px 32px rgba(0,0,0,0.5)" }}>
              {/* Chrome bar */}
              <div style={{ background: "#1c2128", padding: "10px 14px", display: "flex", alignItems: "center", gap: "6px", borderBottom: `1px solid ${C.border}` }}>
                <span style={{ width: 12, height: 12, borderRadius: "50%", background: "#ff5f57", display: "block" }} />
                <span style={{ width: 12, height: 12, borderRadius: "50%", background: "#febc2e", display: "block" }} />
                <span style={{ width: 12, height: 12, borderRadius: "50%", background: "#28c840", display: "block" }} />
                <span style={{ color: C.muted, fontSize: "11px", marginLeft: "8px", fontFamily: "monospace" }}>yourusername — GitHub Profile README</span>
              </div>

              {/* Neofetch layout */}
              <div style={{ padding: "20px 22px", display: "flex", gap: "26px", overflowX: "auto" }}>
                {/* ASCII art column */}
                <pre style={{ margin: 0, color: C.blue, lineHeight: 1.4, fontSize: "11px", fontFamily: "'Cascadia Code', 'Courier New', monospace", flexShrink: 0, userSelect: "none" }}>
{`   ██████████████
 ██              ██
██  ▓▓        ▓▓  ██
██    ████████    ██
██      ████      ██
██  ▓▓        ▓▓  ██
 ██    ████████  ██
   ██████████████

  ← Your photo
    as ASCII art
    goes here`}
                </pre>

                {/* Info column */}
                <div style={{ fontFamily: "'Cascadia Code', 'Courier New', monospace", fontSize: "11.5px", lineHeight: 1.65 }}>
                  <div>
                    <span style={{ color: C.blueBright, fontWeight: "bold" }}>yourusername</span>
                    <span style={{ color: C.textDim }}>@</span>
                    <span style={{ color: C.green, fontWeight: "bold" }}>github</span>
                  </div>
                  <div style={{ color: "#2d333b", marginBottom: "5px" }}>──────────────────────────────────────</div>

                  {[
                    ["OS", "Kali Linux / Windows 11", C.orange],
                    ["Uptime", "24 years, 2 months, 15 days", C.textDim],
                    ["Host", "Bahria University, Islamabad", C.textDim],
                    ["Kernel", "Cybersecurity & IoT Security", C.textDim],
                    ["IDE", "VS Code 1.99.0, PyCharm", C.textDim],
                  ].map(([k, v, c]) => (
                    <div key={k}><span style={{ color: C.green }}>{k}</span><span style={{ color: C.textDim }}>: </span><span style={{ color: c }}>{v}</span></div>
                  ))}

                  <div style={{ color: "#2d333b", margin: "5px 0" }}>──────────────────────────────────────</div>
                  {[
                    ["Languages.Programming", "Python, C/C++, Java"],
                    ["Languages.Computer", "HTML, CSS, SQL, Bash"],
                    ["Languages.Real", "Urdu, English"],
                  ].map(([k, v]) => (
                    <div key={k}><span style={{ color: C.green }}>{k}</span><span style={{ color: C.textDim }}>: {v}</span></div>
                  ))}

                  <div style={{ color: "#2d333b", margin: "5px 0" }}>──────────────────────────────────────</div>
                  {[
                    ["Hobbies.Cyber", "CTFs, Penetration Testing"],
                    ["Hobbies.Dev", "IoT Security, Open Source"],
                  ].map(([k, v]) => (
                    <div key={k}><span style={{ color: C.green }}>{k}</span><span style={{ color: C.textDim }}>: {v}</span></div>
                  ))}

                  <div style={{ color: "#2d333b", margin: "5px 0" }}>──────────────────────────────────────</div>
                  <div style={{ color: C.green }}>Contact</div>
                  {[["Email","you@email.com"],["LinkedIn","linkedin.com/in/yourusername"],["GitHub","github.com/yourusername"]].map(([k, v]) => (
                    <div key={k} style={{ paddingLeft: "12px" }}>
                      <span style={{ color: C.blue }}>{k}</span>
                      <span style={{ color: C.textDim }}>: {v}</span>
                    </div>
                  ))}

                  <div style={{ color: "#2d333b", margin: "5px 0" }}>──────────────────────────────────────</div>
                  <div style={{ color: C.green }}>GitHub Stats</div>
                  {[["★ Stars","42"],["Commits (2025)","183"],["Followers","29"]].map(([k, v]) => (
                    <div key={k} style={{ paddingLeft: "12px" }}>
                      <span style={{ color: C.orange }}>{k}</span>
                      <span style={{ color: C.textDim }}>: {v}</span>
                    </div>
                  ))}

                  {/* Color swatch row */}
                  <div style={{ marginTop: "10px", display: "flex", gap: "3px" }}>
                    {["#161b22","#1f6feb","#388bfd","#58a6ff","#79c0ff","#cae8ff","#f0f6fc"].map(c => (
                      <span key={c} style={{ width: 17, height: 17, background: c, display: "inline-block", borderRadius: "3px", border: `1px solid ${C.border}` }} />
                    ))}
                  </div>
                </div>
              </div>
            </div>

            <div style={{ marginTop: "10px", background: "#0c2d12", border: "1px solid #2ea043", borderRadius: "6px", padding: "11px 14px", fontSize: "12px", color: C.textDim }}>
              <span style={{ color: "#3fb950", fontWeight: 700 }}>How it works: </span>
              This is a monospace code block in Markdown (<code style={{ background: C.el, padding: "1px 5px", borderRadius: "3px", color: C.orange }}>```</code>). The two-column look comes from precise spacing — left column is your ASCII art, right column starts after a gap. GitHub renders it in a fixed-width font, so spacing controls alignment.
            </div>
          </div>
        )}

        {/* ── STEPS ── */}
        {tab === "steps" && (
          <div style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
            {steps.map((step, i) => (
              <div key={i} style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: "8px", padding: "14px 16px", display: "flex", gap: "14px", alignItems: "flex-start" }}>
                <div style={{ width: 36, height: 36, borderRadius: "50%", background: "#1f6feb20", border: "1px solid #1f6feb", display: "flex", alignItems: "center", justifyContent: "center", fontSize: "17px", flexShrink: 0 }}>
                  {step.emoji}
                </div>
                <div>
                  <div style={{ fontWeight: 700, color: C.blue, fontSize: "13px", marginBottom: "8px" }}>
                    Step {i + 1}: {step.title}
                  </div>
                  <ul style={{ margin: 0, padding: "0 0 0 16px", color: C.muted, fontSize: "12px", lineHeight: 1.8 }}>
                    {step.items.map((item, j) => <li key={j}>{item}</li>)}
                  </ul>
                </div>
              </div>
            ))}

            <div style={{ background: "#1a1000", border: `1px solid ${C.orange}30`, borderRadius: "6px", padding: "12px 14px", fontSize: "12px" }}>
              <span style={{ color: C.orange, fontWeight: 700 }}>⚠️ Alignment tip: </span>
              <span style={{ color: C.textDim }}>
                After pasting your ASCII art, count its width (e.g. 22 chars). Add 4–6 spaces of gap. Every right-column line starts at the same character position. If columns look crooked on GitHub, adjust spaces until they line up — monospace makes this precise.
              </span>
            </div>
          </div>
        )}

        {/* ── TEMPLATE ── */}
        {tab === "template" && (
          <div>
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: "8px", overflow: "hidden" }}>
              <div style={{ background: "#1c2128", padding: "10px 16px", display: "flex", justifyContent: "space-between", alignItems: "center", borderBottom: `1px solid ${C.border}` }}>
                <span style={{ color: C.muted, fontSize: "11px", fontFamily: "monospace" }}>README.md — copy and customize</span>
                <button
                  onClick={copyTemplate}
                  style={{ background: copied ? "#238636" : C.el, color: "#fff", border: `1px solid ${copied ? "#2ea043" : C.border}`, borderRadius: "6px", padding: "5px 14px", cursor: "pointer", fontSize: "12px", fontWeight: 700, transition: "all 0.2s" }}
                >
                  {copied ? "✓ Copied!" : "📋 Copy Template"}
                </button>
              </div>
              <pre style={{ margin: 0, padding: "16px", fontFamily: "'Cascadia Code', 'Courier New', monospace", fontSize: "10.5px", color: C.text, overflowX: "auto", overflowY: "auto", maxHeight: "400px", whiteSpace: "pre", lineHeight: 1.65 }}>
                {MARKDOWN_TEMPLATE}
              </pre>
            </div>

            {/* Checklist */}
            <div style={{ marginTop: "12px", background: C.card, border: `1px solid ${C.border}`, borderRadius: "8px", padding: "14px 16px" }}>
              <div style={{ color: C.blue, fontWeight: 700, fontSize: "13px", marginBottom: "10px" }}>📌 Before committing — checklist</div>
              {[
                ["Replace [PASTE YOUR ASCII ART HERE] with your actual ASCII art"],
                ["Fill in real values: OS, uptime (age), university, IDE, languages, hobbies, contact"],
                ["Replace YOUR_USERNAME in the stats card URLs (appears 4 times)"],
                ["Delete the help-text lines inside the left column"],
                ["Check alignment in GitHub's preview before final commit"],
              ].map((item, i) => (
                <div key={i} style={{ display: "flex", gap: "8px", alignItems: "flex-start", padding: "4px 0", color: C.muted, fontSize: "12px" }}>
                  <span style={{ color: C.green, flexShrink: 0, marginTop: "1px" }}>☐</span>
                  <span>{item}</span>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ── TOOLS ── */}
        {tab === "tools" && (
          <div style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
            {/* Resources list */}
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: "8px", overflow: "hidden" }}>
              <div style={{ background: "#1c2128", padding: "10px 16px", borderBottom: `1px solid ${C.border}`, fontSize: "12px", color: C.muted, fontWeight: 600 }}>
                🔗 Essential tools
              </div>
              {tools.map((tool, i) => (
                <div key={i} style={{ padding: "12px 16px", borderBottom: i < tools.length - 1 ? `1px solid ${C.border}` : "none", display: "flex", justifyContent: "space-between", alignItems: "center", gap: "12px" }}>
                  <div>
                    <div style={{ display: "flex", alignItems: "center", gap: "7px", marginBottom: "3px" }}>
                      <span style={{ color: C.text, fontSize: "13px", fontWeight: 600 }}>{tool.name}</span>
                      <span style={{ background: (TAG_COLORS[tool.tag] || C.blue) + "20", color: TAG_COLORS[tool.tag] || C.blue, border: `1px solid ${(TAG_COLORS[tool.tag] || C.blue)}40`, borderRadius: "4px", padding: "1px 6px", fontSize: "10px", fontWeight: 600 }}>{tool.tag}</span>
                    </div>
                    <div style={{ color: C.muted, fontSize: "11px" }}>{tool.desc}</div>
                    {tool.section && <div style={{ color: C.blue, fontSize: "10px", fontFamily: "monospace", marginTop: "2px" }}>{tool.section}</div>}
                  </div>
                </div>
              ))}
            </div>

            {/* Badge snippets */}
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: "8px", overflow: "hidden" }}>
              <div style={{ background: "#1c2128", padding: "10px 16px", borderBottom: `1px solid ${C.border}`, fontSize: "12px", color: C.muted, fontWeight: 600 }}>
                🏷 Ready-to-use skill badges — click to copy
              </div>
              <div style={{ padding: "14px 16px", display: "flex", flexWrap: "wrap", gap: "8px" }}>
                {BADGE_SNIPPETS.map((b) => (
                  <button
                    key={b.label}
                    onClick={() => copyBadge(b.code, b.label)}
                    style={{ background: copiedBadge === b.label ? "#238636" : C.el, color: copiedBadge === b.label ? "#fff" : C.blue, border: `1px solid ${copiedBadge === b.label ? "#2ea043" : C.border}`, borderRadius: "6px", padding: "5px 12px", cursor: "pointer", fontSize: "12px", fontWeight: 500, transition: "all 0.15s" }}
                  >
                    {copiedBadge === b.label ? "✓ Copied" : b.label}
                  </button>
                ))}
              </div>
              <div style={{ padding: "0 16px 14px", color: C.muted, fontSize: "11px" }}>
                More at <span style={{ color: C.blue }}>shields.io</span> — search any language, tool, or platform. Use <code style={{ background: "#0d1117", padding: "1px 5px", borderRadius: "3px" }}>style=for-the-badge</code> for the big style shown in the profile.
              </div>
            </div>

            {/* Bonus tip */}
            <div style={{ background: "#0c1a2e", border: "1px solid #1f6feb40", borderRadius: "8px", padding: "14px 16px", fontSize: "12px" }}>
              <div style={{ color: C.blue, fontWeight: 700, marginBottom: "6px" }}>💡 Pro tip: dynamic uptime</div>
              <div style={{ color: C.textDim, lineHeight: 1.7 }}>
                To auto-update the "Commits" and "Stars" count, use <span style={{ color: C.blue }}>github-readme-stats</span> cards (already in the template). For the neofetch static block, just update the numbers manually when you notice them — it's a developer README, not a live dashboard.
              </div>
            </div>
          </div>
        )}

      </div>
    </div>
  );
}
