import { useState, useEffect } from "react";

const USERNAME = "JooooMohamed";
const REPOS = [
  "firsttech-hackathon-404-team-not-found",
  "task-tracker-fullstack",
  "TB-GOBRA",
  "Euth",
  "MovieSelector",
];

const EMOJI_MAP = {
  "firsttech-hackathon-404-team-not-found": "🏆",
  "task-tracker-fullstack": "✅",
  "TB-GOBRA": "🧬",
  "Euth": "⚡",
  "MovieSelector": "🎬",
};

function getLangBadgeColor(lang) {
  const m = { JavaScript:"yellow", TypeScript:"blue", Python:"blue", Dart:"teal", HTML:"red", CSS:"purple", Java:"orange" };
  return m[lang] || "grey";
}

export default function App() {
  const [repos, setRepos] = useState([]);
  const [loading, setLoading] = useState(true);
  const [copied, setCopied] = useState(false);
  const [tab, setTab] = useState("preview");

  useEffect(() => {
    Promise.all(
      REPOS.map(name =>
        fetch(`https://api.github.com/repos/${USERNAME}/${name}`)
          .then(r => r.json())
          .catch(() => ({ name, description: "", html_url: `https://github.com/${USERNAME}/${name}`, language: "", stargazers_count: 0, forks_count: 0 }))
      )
    ).then(data => { setRepos(data); setLoading(false); });
  }, []);

  const generateReadme = () => {
    if (!repos.length) return "";

    const projectCards = repos.map(r => {
      const em = EMOJI_MAP[r.name] || "🔧";
      const desc = r.description || "A cool project by JooooMohamed.";
      const lang = r.language || "";
      const badgeColor = getLangBadgeColor(lang);
      const langBadge = lang ? `![${lang}](https://img.shields.io/badge/-${encodeURIComponent(lang)}-${badgeColor}?style=flat-square&logo=${lang.toLowerCase().replace(/\s/g,'')}&logoColor=white)` : "";
      return `### ${em} [${r.name}](${r.html_url})
> ${desc}

${langBadge} ![Stars](https://img.shields.io/github/stars/${USERNAME}/${r.name}?style=flat-square&color=6C63FF) ![Forks](https://img.shields.io/github/forks/${USERNAME}/${r.name}?style=flat-square&color=orange)

[![View Repo](https://img.shields.io/badge/View%20Repo-6C63FF?style=for-the-badge&logo=github&logoColor=white)](${r.html_url})

---`;
    }).join("\n\n");

    return `<div align="center">

<!-- Animated Header -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=28&pause=1000&color=6C63FF&center=true&vCenter=true&width=650&lines=Hey+there!+I'm+Mohamed+👋;Full+Stack+Dev+%7C+Cloud+%7C+Mobile;Building+cool+stuff+one+commit+at+a+time+🚀;404%3A+Boredom+Not+Found+😄" alt="Typing SVG" />

<br/>

![Profile Views](https://komarev.com/ghpvc/?username=${USERNAME}&style=for-the-badge&color=6C63FF)
[![GitHub followers](https://img.shields.io/github/followers/${USERNAME}?style=for-the-badge&color=6C63FF&logo=github)](https://github.com/${USERNAME})

</div>

---

## 🧑‍💻 About Me

\`\`\`typescript
const mohamed = {
  role      : "Full Stack Developer 🚀",
  stack     : ["JavaScript", "TypeScript", "React", "Node.js"],
  mobile    : ["React Native", "Flutter", "Expo"],
  cloud     : ["AWS", "Docker", "GitHub Actions", "CI/CD"],
  learning  : "Always something new 👀",
  funFact   : "I push to main and pray 🙏",
};
\`\`\`

---

## 🛠️ Tech Stack

### 💻 Languages & Frameworks
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)

### 📱 Mobile
![React Native](https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Expo](https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white)

### ☁️ DevOps & Cloud
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonwebservices&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

### 🗄️ Databases & Tools
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![VS Code](https://img.shields.io/badge/VS_Code-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white)

---

## 📊 GitHub Stats

<div align="center">

<img height="180em" src="https://github-readme-stats.vercel.app/api?username=${USERNAME}&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true&hide_border=true"/>
<img height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=${USERNAME}&layout=compact&langs_count=7&theme=tokyonight&hide_border=true"/>

</div>

<div align="center">

[![GitHub Streak](https://streak-stats.demolab.com?user=${USERNAME}&theme=tokyonight&hide_border=true)](https://git.io/streak-stats)

</div>

---

## 🚀 Featured Projects

${projectCards}

## 🎯 What I'm Up To

- 🔭 Working on full-stack web & mobile apps
- 🌱 Exploring cloud-native architectures & DevOps
- 💬 Ask me about **React Native, TypeScript, or CI/CD**
- 🏆 Hackathon enthusiast — love building under pressure!
- ⚡ Fun fact: Coffee → Code → Deploy → Repeat ☕

---

## 🤝 Connect With Me

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-${USERNAME}-181717?style=for-the-badge&logo=github)](https://github.com/${USERNAME})

</div>

---

<div align="center">

*"First, solve the problem. Then, write the code."* — John Johnson

![Wave](https://raw.githubusercontent.com/mayhemantt/mayhemantt/Update/svg/Bottom.svg)

</div>`;
  };

  const md = generateReadme();

  const copy = () => {
    navigator.clipboard.writeText(md).then(() => {
      setCopied(true);
      setTimeout(() => setCopied(false), 2500);
    });
  };

  if (loading) return (
    <div style={{ display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", height:"100vh", background:"#0d1117", color:"#c9d1d9", fontFamily:"monospace", gap:16 }}>
      <div style={{ fontSize:40 }}>⏳</div>
      <div style={{ fontSize:16 }}>Fetching your 5 repos from GitHub...</div>
    </div>
  );

  return (
    <div style={{ background:"#0d1117", minHeight:"100vh", color:"#c9d1d9", fontFamily:"'Segoe UI', sans-serif" }}>
      {/* Header */}
      <div style={{ background:"#161b22", borderBottom:"1px solid #30363d", padding:"16px 24px", display:"flex", alignItems:"center", justifyContent:"space-between", flexWrap:"wrap", gap:12, position:"sticky", top:0, zIndex:10 }}>
        <div>
          <h1 style={{ margin:0, color:"#6C63FF", fontSize:20 }}>✨ Your Enhanced README</h1>
          <p style={{ margin:"2px 0 0", color:"#8b949e", fontSize:12 }}>5 projects loaded · Ready to push!</p>
        </div>
        <div style={{ display:"flex", gap:8, flexWrap:"wrap" }}>
          {["preview","markdown"].map(t => (
            <button key={t} onClick={() => setTab(t)} style={{
              background: tab===t ? "#6C63FF" : "#21262d",
              color: tab===t ? "#fff" : "#8b949e",
              border:"none", borderRadius:8, padding:"8px 16px",
              cursor:"pointer", fontWeight:600, fontSize:13,
            }}>
              {t==="preview" ? "👁 Preview" : "📋 Markdown"}
            </button>
          ))}
          <button onClick={copy} style={{
            background: copied ? "#238636" : "#f0883e",
            color:"#fff", border:"none", borderRadius:8,
            padding:"8px 18px", cursor:"pointer", fontWeight:700, fontSize:13,
          }}>
            {copied ? "✅ Copied!" : "⬇ Copy README"}
          </button>
        </div>
      </div>

      {/* Preview tab */}
      {tab === "preview" && (
        <div style={{ padding:24, maxWidth:860, margin:"0 auto" }}>
          <div style={{ background:"#161b22", border:"1px solid #30363d", borderRadius:12, padding:28 }}>

            <div style={{ textAlign:"center", marginBottom:28 }}>
              <div style={{ background:"linear-gradient(135deg,#6C63FF,#3178c6)", borderRadius:12, padding:"20px 24px", marginBottom:16 }}>
                <h2 style={{ margin:0, color:"#fff", fontSize:22 }}>👋 Hey there! I'm Mohamed</h2>
                <p style={{ margin:"6px 0 0", color:"rgba(255,255,255,0.8)", fontSize:14 }}>Full Stack Dev · Cloud Enthusiast · Mobile Builder</p>
              </div>
              <code style={{ background:"#21262d", padding:"6px 14px", borderRadius:20, fontSize:12, color:"#8b949e" }}>
                Building cool stuff one commit at a time 🚀
              </code>
            </div>

            <h3 style={{ color:"#c9d1d9", borderBottom:"1px solid #30363d", paddingBottom:8 }}>🚀 Featured Projects</h3>
            <div style={{ display:"flex", flexDirection:"column", gap:16 }}>
              {repos.map(r => (
                <div key={r.name} style={{ background:"#0d1117", border:"1px solid #30363d", borderRadius:10, padding:18, display:"flex", gap:16, alignItems:"flex-start" }}>
                  <span style={{ fontSize:28, flexShrink:0 }}>{EMOJI_MAP[r.name] || "🔧"}</span>
                  <div style={{ flex:1, minWidth:0 }}>
                    <a href={r.html_url} target="_blank" rel="noreferrer" style={{ color:"#58a6ff", fontWeight:700, fontSize:16, textDecoration:"none" }}>{r.name}</a>
                    <p style={{ margin:"4px 0 8px", color:"#8b949e", fontSize:13 }}>{r.description || "A cool project by JooooMohamed."}</p>
                    <div style={{ display:"flex", gap:10, flexWrap:"wrap", fontSize:12 }}>
                      {r.language && <span style={{ background:"#21262d", padding:"2px 10px", borderRadius:20, color:"#c9d1d9" }}>{r.language}</span>}
                      <span style={{ color:"#8b949e" }}>⭐ {r.stargazers_count}</span>
                      <span style={{ color:"#8b949e" }}>🍴 {r.forks_count}</span>
                    </div>
                  </div>
                </div>
              ))}
            </div>

            <div style={{ marginTop:24, padding:16, background:"#0d1117", borderRadius:10, border:"1px solid #30363d" }}>
              <p style={{ margin:0, color:"#8b949e", fontSize:13, textAlign:"center" }}>
                📊 Stats cards, streak counter, animated typing header, tech badges & wave footer are all included in the markdown!
              </p>
            </div>
          </div>
        </div>
      )}

      {/* Markdown tab */}
      {tab === "markdown" && (
        <div style={{ padding:24 }}>
          <p style={{ color:"#8b949e", fontSize:13, marginTop:0 }}>
            Copy this and paste it into your <code style={{ color:"#58a6ff", background:"#21262d", padding:"2px 6px", borderRadius:4 }}>JooooMohamed/README.md</code> file, then commit & push.
          </p>
          <div style={{
            background:"#161b22", border:"1px solid #30363d", borderRadius:10,
            padding:20, fontSize:12, lineHeight:1.8, color:"#c9d1d9",
            whiteSpace:"pre-wrap", wordBreak:"break-word",
            fontFamily:"'Fira Code','Consolas',monospace",
            userSelect:"all", maxHeight:"70vh", overflowY:"auto",
          }}>
            {md}
          </div>
          <p style={{ color:"#8b949e", fontSize:12, marginTop:10 }}>
            💡 Press <kbd style={{ background:"#21262d", padding:"1px 5px", borderRadius:3 }}>Ctrl+A</kbd> inside the box then <kbd style={{ background:"#21262d", padding:"1px 5px", borderRadius:3 }}>Ctrl+C</kbd> — or hit <strong style={{ color:"#f0883e" }}>⬇ Copy README</strong> above.
          </p>
        </div>
      )}
    </div>
  );
}
