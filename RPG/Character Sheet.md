# 🛡️ Лист персонажа

```dataviewjs
// Получаем все ежедневные заметки из папки Daily 
const pages = dv.pages('"Daily"');

let totalSTR = 0; let totalINT = 0; let totalCHA = 0; let totalWIS = 0;

for (let p of pages) { if (p.xp_str) totalSTR += Number(p.xp_str); if (p.xp_int) totalINT += Number(p.xp_int); if (p.xp_cha) totalCHA += Number(p.xp_cha); if (p.xp_wis) totalWIS += Number(p.xp_wis); }

const totalXP = totalSTR + totalINT + totalCHA + totalWIS; const totalGold = Math.floor(totalXP / 10);

function getLevelData(xp) { const xpPerLevel = 500; const level = Math.floor(xp / xpPerLevel) + 1; const currentXp = xp % xpPerLevel; const percent = Math.round((currentXp / xpPerLevel) * 100); return { level, currentXp, xpPerLevel, percent }; }

const stats = [ { name: "💪 Сила (STR)", data: getLevelData(totalSTR), total: totalSTR }, { name: "🧠 Интеллект (INT)", data: getLevelData(totalINT), total: totalINT }, { name: "🗣️ Харизма (CHA)", data: getLevelData(totalCHA), total: totalCHA }, { name: "🧘 Мудрость (WIS)", data: getLevelData(totalWIS), total: totalWIS } ];

let tableData = stats.map(s => [ s.name, `**Ур. ${s.data.level}**`, `${s.data.currentXp} / ${s.data.xpPerLevel} XP`, `${s.data.percent}%`, `${s.total} XP` ]);

dv.header(3, `💰 Накоплено золота: ${totalGold} Gold (Всего XP: ${totalXP})`); dv.table(["Характеристика", "Уровень", "До след. уровня", "Прогресс", "Всего XP"], tableData);
```


