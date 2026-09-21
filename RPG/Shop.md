# 🛒 Лавка наград

```dataviewjs
// 1. Считаем заработанное золото со всех дней в папке Daily
const pages = dv.pages('"Daily"');
let totalXP = 0;

for (let p of pages) {
    if (p.xp_str) totalXP += Number(p.xp_str);
    if (p.xp_int) totalXP += Number(p.xp_int);
    if (p.xp_cha) totalXP += Number(p.xp_cha);
    if (p.xp_wis) totalXP += Number(p.xp_wis);
}

const earnedGold = Math.floor(totalXP / 10);

// 2. Считаем потраченное золото из журнала покупок ниже
const currentFile = await app.vault.read(app.workspace.getActiveFile());
const lines = currentFile.split("\n");

let spentGold = 0;
let isLogSection = false;

for (let line of lines) {
    if (line.includes("### 🧾 Журнал покупок")) {
        isLogSection = true;
        continue;
    }
    if (isLogSection && line.startsWith("|")) {
        const parts = line.split("|").map(s => s.trim());
        // Проверяем строку таблицы с ценой: | Дата | Награда | Стоимость |
        if (parts.length >= 4) {
            const cost = parseInt(parts[3], 10);
            if (!isNaN(cost)) {
                spentGold += cost;
            }
        }
    }
}

const currentBalance = earnedGold - spentGold;
const balanceColor = currentBalance >= 0 ? "lightgreen" : "salmon";

// 3. Выводим карточку баланса
dv.paragraph(`
> [!money] ### 💰 Финансы героя
> * **Всего заработано:** ${earnedGold} Gold (из ${totalXP} XP)
> * **Всего потрачено:** ${spentGold} Gold
> * **Текущий остаток:** <span style="font-size: 1.2em; font-weight: bold; color: ${balanceColor};">${currentBalance} Gold</span>
`);
```

---

### 📜 Прейскурант товаров

| Награда | Стоимость | Описание / Условия |
| :--- | :---: | :--- |
| ☕ **Чашка спешелти кофе / десерт** | **40 Gold** | В любимой кофейне |
| 🎮 **1 час видеоигр / 1 серия сериала** | **50 Gold** | Полный отдых без чувства вины |
| 🍕 **Читмил / пицца / бургер** | **150 Gold** | Вкусная еда в выходной |
| 📚 **Новая книга** | **300 Gold** | Бумажное или электронное издание |
| 🎬 **Билет в кино / развлечение** | **400 Gold** | Поход на премьеру |
| 👕 **Одежда / мерч** | **1 000 Gold** | Вещь в гардероб |
| 🎧 **Техника / гаджет** | **2 500 Gold** | Крупная желанная покупка |

---

### 🧾 Журнал покупок

> Когда покупаете награду, просто дописывайте новую строчку в таблицу ниже. Баланс вверху пересчитается сам.

| Дата | Награда | Стоимость |
| :--- | :------ | :-------- |
|      |         |           |
