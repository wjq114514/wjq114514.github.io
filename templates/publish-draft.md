<%*
const file = app.workspace.getActiveFile();
if (!file) {
  new Notice("❌ 没有打开任何文件");
  return;
}

const filePath = file.path;
new Notice("📄 当前文件路径：" + filePath);

if (!filePath.startsWith("_drafts/")) {
  new Notice("❌ 当前文件不在 _drafts/ 目录下");
  return;
}

const confirm = await tp.system.suggester(
  ["✅ 确认发布", "❌ 取消"],
  [true, false],
  false,
  "将发布此草稿到 _posts/"
);
if (!confirm) return;

const content = await app.vault.read(file);
const newContent = content.replace(/^draft:\s*true\s*\n/im, "");
const newPath = filePath.replace("_drafts/", "_posts/");

await app.vault.create(newPath, newContent);
await app.vault.delete(file);

const newFile = app.vault.getAbstractFileByPath(newPath);
await app.workspace.getLeaf().openFile(newFile);

new Notice("✅ 已发布到 _posts/！");
%>