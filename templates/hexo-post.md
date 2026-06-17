---
title: <% tp.file.title %>
date: <% tp.date.now("YYYY-MM-DD HH:mm:ss") %>
updated: <% tp.date.now("YYYY-MM-DD HH:mm:ss") %>
<%*
const type = await tp.system.suggester(
  ["📝 正式文章 → _posts/", "📄 草稿 → _drafts/"],
  ["post", "draft"]
);

let frontmatter = "";

if (type === "draft") {
  await tp.file.move("_drafts/" + tp.file.title);
  frontmatter += "draft: true\n";
  frontmatter += "categories: []\n";
  frontmatter += "tags: []\n";
} else {
  const catInput = await tp.system.prompt("📂 分类（留空则不填）");
  const tagInput = await tp.system.prompt("🏷️ 标签，逗号分隔（留空则不填）");

  frontmatter += "categories:\n";
  if (catInput) {
    frontmatter += "  - " + catInput + "\n";
  } else {
    frontmatter += "  - \n";
  }

  frontmatter += "tags:\n";
  if (tagInput) {
    const tags = tagInput.split(",").map(t => t.trim()).filter(t => t);
    for (let tag of tags) {
      frontmatter += "  - " + tag + "\n";
    }
  } else {
    frontmatter += "  - \n";
  }
}

frontmatter += "cover: \n";
frontmatter += "excerpt: \n";

tR += frontmatter;
%>
---