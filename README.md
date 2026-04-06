import { useState, useEffect } from "react";

const USERNAME = "JooooMohamed";

function getLangColor(lang) {
  const colors = {
    JavaScript: "#f1e05a", TypeScript: "#3178c6", Python: "#3572A5",
    Dart: "#00B4AB", HTML: "#e34c26", CSS: "#563d7c", Java: "#b07219",
    "C++": "#f34b7d", Shell: "#89e051", Go: "#00ADD8",
  };
  return colors[lang] || "#8b949e";
}

export default function App() {
  const [repos, setRepos] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [selected, setSelected] = useState([]);
  const [copied, setCopied] = useState(false);
  const [tab, setTab] = useState("pick"); // "pick" | "markdown"

  useEffect(() => {
    fetch(`https://api.github.com/users/${USERNAME}/repos?per_page=100&sort=updated`)
      .then(r => r.json())
      .then(data => {
        if (data.message) throw new Error(data.message);
        const filtered = data.filter(r => !r.fork).sort((a, b) => b.stargazers_count - a.stargazers_count);
        setRepos(filtered);
        setSelected(filtered.slice(0, 6).map(r => r.id));
        setLoading(false);
      })
      .catch(e => { setError(e.message); setLoading(false); });
  }, []);

  const toggle = (id) => {
    setSelected(prev =>
      prev.includes(id)
        ? prev.filter(x => x !== id)
        : prev.length < 6 ? [...prev, id] : prev
    );
  };

  const generateMarkdown = () => {
    const picked = repos.filter(r => selected.includes(r.id));
    if (!picked.length) return "";
    const cards = picked.map(r =>
      `[![${r.name}](https://github-readme-stats.vercel.app/api/pin/?username=${USERNAME}&repo=${encodeURIComponent(r.name)}&theme=tokyonight&hide_border=true)](${r.html_url})`
    );
    const rows = [];
    for (let i = 0; i < cards.length; i += 2) rows.push(cards.slice(i, i + 2).join("\n"));
    return `## 🚀 Featured Projects\n\n<div align="center">\n\n${rows.join("\n\n")}\n\n</div>`;
  };

  const md = generateMarkdown();

  const copyMd = () => {
    navigator.clipboard.writeText(md).then(() => {
      setCopied(true);
      setTimeout(() => setCopied(false), 2500);
    });
  };

  if (loading) return (
    <div style={{ display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", height:"100vh", background:"#0d1117", color:"#c9d1d9", fontFamily:"monospace", gap:12 }}>
      <div style={{ fontSize:32 }}>⏳</div>
      <div>Fetching your repos from GitHub...</div>
    </div>
  );

  if (error) return (
    <div style={{ display:"flex", alignItems:"center", justifyContent:"center", height:"100vh", background:"#0d1117", color:"#f85149", fontFamily:"monospace", padding:24, textAlign:"center" }}>
      ❌ Error: {error}
    </div>
  );

  return (
    <div style={{ background:"#0d1117", minHeight:"100vh", color:"#c9d1d9", fontFamily:"'Segoe UI', sans-serif" }}>
      {/* Header */}
      <div style={{ background:"#161b22", borderBottom:"1px solid #30363d", padding:"16px 24px", display:"flex", alignItems:"center", justifyContent:"space-between", flexWrap:"wrap", gap:12 }}>
        <div>
          <h1 style={{ margin:0, color:"#6C63FF", fontSize:20 }}>🚀 Project Showcase Picker</h1>
          <p style={{ margin:"2px 0 0", color:"#8b949e", fontSize:13 }}>
            {repos.length} repos found · {selected.length}/6 selected
          </p>
        </div>
        {/* Tabs */}
        <div style={{ display:"flex", gap:8 }}>
          {["pick","markdown"].map(t => (
            <button key={t} onClick={() => setTab(t)} style={{
              background: tab === t ? "#6C63FF" : "#21262d",
              color: tab === t ? "#fff" : "#8b949e",
              border:"none", borderRadius:8, padding:"8px 18px",
              cursor:"pointer", fontWeight:600, fontSize:14,
            }}>
              {t === "pick" ? "📁 Pick Projects" : "📋 View Markdown"}
            </button>
          ))}
        </div>
      </div>

      {/* Pick tab */}
      {tab === "pick" && (
        <div style={{ padding:24 }}>
          <p style={{ color:"#8b949e", fontSize:13, margin:"0 0 16px" }}>Click a card to select it (max 6). Then switch to <strong style={{color:"#c9d1d9"}}>View Markdown</strong> to copy the code.</p>
          <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill, minmax(250px, 1fr))", gap:12 }}>
            {repos.map(r => {
              const isSel = selected.includes(r.id);
              return (
                <div key={r.id} onClick={() => toggle(r.id)} style={{
                  background: isSel ? "#1a1f35" : "#161b22",
                  border: `2px solid ${isSel ? "#6C63FF" : "#30363d"}`,
                  borderRadius:10, padding:"14px 16px", cursor:"pointer",
                  transition:"border-color .15s", position:"relative",
                }}>
                  {isSel && (
                    <span style={{ position:"absolute", top:10, right:12, background:"#6C63FF", color:"#fff", borderRadius:"50%", width:20, height:20, display:"flex", alignItems:"center", justifyContent:"center", fontSize:12, fontWeight:700 }}>✓</span>
                  )}
                  <div style={{ fontWeight:700, color:"#58a6ff", fontSize:14, paddingRight:24, marginBottom:6, wordBreak:"break-all" }}>{r.name}</div>
                  <div style={{ fontSize:12, color:"#8b949e", marginBottom:10, minHeight:30 }}>{r.description || "No description"}</div>
                  <div style={{ display:"flex", gap:12, fontSize:11, color:"#8b949e" }}>
                    {r.language && (
                      <span style={{ display:"flex", alignItems:"center", gap:4 }}>
                        <span style={{ width:9, height:9, borderRadius:"50%", background:getLangColor(r.language), display:"inline-block" }}/>
                        {r.language}
                      </span>
                    )}
                    <span>⭐ {r.stargazers_count}</span>
                    <span>🍴 {r.forks_count}</span>
                  </div>
                </div>
              );
            })}
          </div>
        </div>
      )}

      {/* Markdown tab */}
      {tab === "markdown" && (
        <div style={{ padding:24 }}>
          {selected.length === 0 ? (
            <div style={{ color:"#8b949e", textAlign:"center", marginTop:60, fontSize:16 }}>
              👈 Go to <strong style={{color:"#c9d1d9"}}>Pick Projects</strong> and select at least one repo first.
            </div>
          ) : (
            <>
              <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:16, flexWrap:"wrap", gap:10 }}>
                <p style={{ margin:0, color:"#8b949e", fontSize:13 }}>
                  Copy this and paste it into your <code style={{color:"#58a6ff", background:"#21262d", padding:"2px 6px", borderRadius:4}}>JooooMohamed/README.md</code>
                </p>
                <button onClick={copyMd} style={{
                  background: copied ? "#238636" : "#6C63FF",
                  color:"#fff", border:"none", borderRadius:8,
                  padding:"10px 22px", cursor:"pointer", fontSize:15, fontWeight:700,
                  display:"flex", alignItems:"center", gap:8,
                }}>
                  {copied ? "✅ Copied!" : "📋 Copy All"}
                </button>
              </div>
              <div style={{
                background:"#161b22", border:"1px solid #30363d", borderRadius:10,
                padding:20, fontSize:13, lineHeight:1.8, color:"#c9d1d9",
                whiteSpace:"pre-wrap", wordBreak:"break-word", fontFamily:"'Fira Code', 'Consolas', monospace",
                overflowX:"auto", userSelect:"all",
              }}>
                {md}
              </div>
              <p style={{ color:"#8b949e", fontSize:12, marginTop:10 }}>💡 You can also click inside the box and press <kbd style={{background:"#21262d", padding:"1px 5px", borderRadius:3}}>Ctrl+A</kbd> → <kbd style={{background:"#21262d", padding:"1px 5px", borderRadius:3}}>Ctrl+C</kbd> to copy manually.</p>
            </>
          )}
        </div>
      )}
    </div>
  );
}
