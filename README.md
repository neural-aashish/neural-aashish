<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>GitHub Contribution Dashboard</title>
<style>
body {
  background: #0d1117;
  color: #c9d1d9;
  font-family: Arial;
  text-align: center;
}

.container {
  width: 90%;
  margin: auto;
}

h1 {
  margin: 20px;
  color: #58a6ff;
}

/* GRID */
.grid {
  display: grid;
  grid-template-columns: repeat(53, 12px);
  gap: 4px;
  justify-content: center;
}

.cell {
  width: 12px;
  height: 12px;
  border-radius: 2px;
  background: #161b22;
}

/* COLORS */
.lvl-0 { background: #161b22; }
.lvl-1 { background: #0e4429; }
.lvl-2 { background: #006d32; }
.lvl-3 { background: #26a641; }
.lvl-4 { background: #39d353; }

/* STATS */
.stats {
  display: flex;
  justify-content: space-around;
  margin: 30px 0;
  padding: 20px;
  border: 1px solid #30363d;
  border-radius: 10px;
}

.card {
  text-align: center;
}

.card h2 {
  color: #58a6ff;
}

</style>
</head>
<body>

<div class="container">
  <h1>GitHub Contribution Dashboard</h1>

  <div class="stats">
    <div class="card">
      <h2 id="total">0</h2>
      <p>Total Contributions</p>
    </div>
    <div class="card">
      <h2 id="streak">0</h2>
      <p>Current Streak</p>
    </div>
    <div class="card">
      <h2 id="maxStreak">0</h2>
      <p>Longest Streak</p>
    </div>
  </div>

  <div class="grid" id="grid"></div>
</div>

<script>
const username = "YOUR_USERNAME";

fetch(`https://github-contributions-api.jogruber.de/v4/${username}`)
.then(res => res.json())
.then(data => {
  const weeks = data.contributions;
  const grid = document.getElementById("grid");

  let total = 0;
  let streak = 0;
  let maxStreak = 0;

  let currentStreak = 0;

  weeks.forEach(week => {
    week.forEach(day => {
      total += day.count;

      const div = document.createElement("div");
      div.classList.add("cell");

      let lvl = 0;
      if (day.count > 0) lvl = 1;
      if (day.count > 3) lvl = 2;
      if (day.count > 6) lvl = 3;
      if (day.count > 10) lvl = 4;

      div.classList.add("lvl-" + lvl);
      grid.appendChild(div);

      // streak logic
      if (day.count > 0) {
        currentStreak++;
        if (currentStreak > maxStreak) maxStreak = currentStreak;
      } else {
        currentStreak = 0;
      }
    });
  });

  streak = currentStreak;

  document.getElementById("total").innerText = total;
  document.getElementById("streak").innerText = streak;
  document.getElementById("maxStreak").innerText = maxStreak;
});
</script>

</body>
</html>
