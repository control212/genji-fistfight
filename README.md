# genji-fistfight
<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>겐지 주먹전 - 규칙</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <h1>겐지 주먹전 규칙</h1>

  <div class="card">
    <h2>📌 기본 규칙</h2>
    <ul>
      <li>주먹 버튼을 눌러 상대에게 데미지를 입힌다.</li>
      <li>상대는 일정 확률로 반격하며 생명이 감소한다.</li>
      <li>생명이 0이 되면 게임 오버.</li>
    </ul>

    <h2>📌 스킬 규칙</h2>
    <ul>
      <li>스킬은 8초 쿨다운이 있다.</li>
      <li>스킬 적중 시 강력한 데미지를 준다.</li>
    </ul>

    <h2>📌 점수 규칙</h2>
    <ul>
      <li>상대 1명을 쓰러뜨릴 때 +100점</li>
      <li>연속 히트 시 보너스 점수 적용 가능</li>
    </ul>

    <h2>❗ 금지 사항 (중요)</h2>
    <ul>
      <li>튕 3번 이상 금지</li>
      <li>튕겨내기 + 근접 공격 금지</li>
      <li>튀는 플레이(도망 플레이) 금지</li>
      <li>나는 플레이(공중 도주/회피) 금지</li>
      <li>대기 플레이(시간 끌기) 금지</li>
      <li>수비만 하는 플레이 금지</li>
      <li>잠수하는 척 속이고 공격하는 플레이 금지</li>
    </ul>
  </div>

</body>
</html>
function autoSaveRanking(playerName, finalScore) {
  const ref = db.ref("ranking").push();
  ref.set({
    name: playerName,
    score: finalScore,
    timestamp: Date.now()
  });
}
if (lives <= 0) {
  messageEl.textContent = '게임 오버!';

  // 자동 랭킹 저장
  const playerName = prompt("닉네임을 입력하세요:");
  autoSaveRanking(playerName, score);
}
<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>겐지 주먹전 - 랭킹</title>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase.js"></script>
  <script src="js/firebase.js"></script>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

<h1>🏆 겐지 주먹전 랭킹</h1>

<table id="rankTable" class="rank">
  <tr>
    <th>순위</th>
    <th>닉네임</th>
    <th>점수</th>
  </tr>
</table>

<script>
  const table = document.getElementById("rankTable");

  // Firebase에 랭킹 데이터가 변경될 때마다 자동 업데이트
  db.ref("ranking").orderByChild("score").limitToLast(100).on("value", snapshot => {

    const data = [];
    snapshot.forEach(item => data.push(item.val()));

    // 높은 점수순으로 정렬
    data.sort((a, b) => b.score - a.score);

    // UI 리셋 후 새 데이터 채움
    table.innerHTML = `
      <tr>
        <th>순위</th>
        <th>닉네임</th>
        <th>점수</th>
      </tr>
    `;

    data.forEach((item, index) => {
      table.innerHTML += `
        <tr>
          <td>${index + 1}</td>
          <td>${item.name}</td>
          <td>${item.score}</td>
        </tr>
      `;
    });
  });
</script>

</body>
</html>