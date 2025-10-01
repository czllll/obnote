```dataviewjs
const domainOrder = ["ai", "java", "linux","db", "algorithm", "frontend", "middleware", "system-design","devops","python"];
const layerOrder = {
  ai: ["llm", "cnn", "transformer"],
  java: ["javase", "jvm", "juc", "jdk", "spring"],
  db: ["mysql", "redis", "es", "cassandra"],
  algorithm: [],
  frontend: ["javascript"],
  middleware: ["mq"],
  "system-design": ["design-pattern"],
  "devops":["maven"],
  "linux":[],
  "python":[]
};

const typeColors = { fact: "#6AB04C", insight: "#F39C12", blog: "#9B59B6" };

const pages = dv.pages("").where(p => Array.isArray(p.file?.tags));

function normalizeTags(tags) {
  if (!Array.isArray(tags)) return [];
  return tags.map(s => (typeof s === "string" ? s : String(s))).map(s => s.trim()).filter(Boolean);
}

function getType(page) {
  let t = page.type ?? page["Type"] ?? page["TYPE"];
  if (typeof t === "string") t = t.trim().toLowerCase();
  if (!t) {
    const tags = normalizeTags(page.file.tags).map(s => s.replace(/^#/, "").toLowerCase());
    const direct = tags.find(x => x === "fact" || x === "insight" || x === "blog");
    if (direct) t = direct;
    if (!t) {
      const typeTag = tags.find(x => x.startsWith("type/"));
      if (typeTag) {
        const cand = typeTag.split("/")[1];
        if (cand === "fact" || cand === "insight" || cand === "blog") t = cand;
      }
    }
  }
  return t || null;
}

function hasTag(page, tag) {
  const tags = normalizeTags(page.file.tags);
  return tags.includes("#" + tag);
}

dv.paragraph("当前共有 " + domainOrder.length + " 个 domain：" + domainOrder.join(", "));

// 一个小工具，用于输出“无表头”的表格（传空表头）
function tableNoHeader(rows) {
  // 传入空数组作为表头，Dataview 会渲染无表头表格
  dv.table([], rows);
}

for (const domain of domainOrder) {
  const layers = Array.isArray(layerOrder[domain]) ? layerOrder[domain] : [];

  // 这些标题在大纲中通常不会出现（因为是 dv.header），仅做页面视觉结构
  dv.header(2, domain);

  for (const layer of layers) {
    dv.header(3, layer);

    const rows = [];
    for (const page of pages) {
      if (hasTag(page, domain) && hasTag(page, layer)) {
        const t = getType(page);
        const color = t && typeColors[t] ? typeColors[t] : "#666";
        const typeDisplay = t ? `<span style="color:${color}; font-weight:600;">${t}</span>` : "(no type)";
        rows.push([dv.fileLink(page.file.path), typeDisplay]);
      }
    }

    if (rows.length > 0) tableNoHeader(rows);
    else dv.paragraph("_该层暂无笔记_");
  }

  // “no layer” 分组
  dv.header(3, "no layer");
  {
    const rows = [];
    for (const page of pages) {
      if (!hasTag(page, domain)) continue;
      const hasAnyLayer = layers.some(l => hasTag(page, l));
      if (!hasAnyLayer) {
        const t = getType(page);
        const color = t && typeColors[t] ? typeColors[t] : "#666";
        const typeDisplay = t ? `<span style="color:${color}; font-weight:600;">${t}</span>` : "(no type)";
        rows.push([dv.fileLink(page.file.path), typeDisplay]);
      }
    }
    if (rows.length > 0) tableNoHeader(rows);
    else dv.paragraph("_该分组暂无笔记_");
  }

  dv.paragraph("<hr>");
}

```