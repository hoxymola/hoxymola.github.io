---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

### 좋아해요 🙂
🐶 강쥐 🎮 게임 ☔️ 비\
🍔 햄버거 🍗 치킨 🍕피자\
☃️ 겨울 😴 드르렁\
<br>

### 싫어해요 🙃
🦟 벌레 🧑‍💻 일 ☀️ 햇빛\
🫘 콩 🍆 가지 🍄‍🟫 버섯\
🏖️ 여름 🚶 뚜벅뚜벅\
<br>

### 여기까지 했어요 😎
[![현재 진행 배지](/assets/class/c2g.svg){: .badge-wrap }](https://solved.ac/class)

{% assign classes = "1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list = "16,22,11,0,0,0,0" | split: "," %}
{% assign total_list = "16,22,40,47,48,48,48" | split: "," %}
{% assign colors = "#249CE5,#20C5DF,#1BDF8B,#2BD521,#B0DB15,#EBCA0F,#F3B312" | split: "," %}
{% assign hover_colors = "#5AB4F5,#4DE1E7,#3CEB9D,#54F75D,#D2E635,#F3D425,#F7C73A" | split: "," %}

<div class="chart-wrapper">
  <div class="chart-container">
    {% for i in (0..6) %}
      {% assign solved = solved_list[i] | plus: 0 %}
      {% assign total = total_list[i] | plus: 0 %}
      {% assign percent = solved | times: 100 | divided_by: total %}
      <div class="chart-item" style="--chart-color: {{ colors[i] }}; --chart-hover-color: {{ hover_colors[i] }}; --percent: {{ percent }}">

        {% if solved > 0 %}
          <svg viewBox="0 0 36 36" class="circular-chart clickable" onclick="location.href='/categories/class-{{ classes[i] }}'">
        {% else %}
          <svg viewBox="0 0 36 36" class="circular-chart">
        {% endif %}

            <path class="circle-bg"
                  d="M18 2.0845
                     a 15.9155 15.9155 0 0 1 0 31.831
                     a 15.9155 15.9155 0 0 1 0 -31.831"/>
            <path class="circle"
                  stroke-dasharray="{{ percent | round: 1 }}, 100"
                  d="M18 2.0845
                     a 15.9155 15.9155 0 0 1 0 31.831
                     a 15.9155 15.9155 0 0 1 0 -31.831"/>
            <text x="18" y="18" class="percentage">
              {{ percent | round: 1 }}%
            </text>
            <text x="18" y="18" class="ratio">
              {{ solved }}/{{ total }}
            </text>
          </svg>
        <div class="chart-title">CLASS {{ classes[i] }}</div>
      </div>
    {% endfor %}
  </div>
</div>

<style>
/* ✅ 1. 아이콘 가운데 정렬 */
.badge-wrap {
  width: 120px;
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.chart-wrapper {
  border: 1px solid var(--text-color);
  border-radius: 16px;
  padding: 24px 20px;
  margin-top: 24px;
  overflow: hidden; /* ✅ 테두리 밖으로 넘침 방지 */
}

/* ===== Grid로 1줄(7개) → 2줄(4+3) ===== */
.chart-container {
  display: grid;
  grid-template-columns: repeat(7, 80px); /* 기본 1줄 7개 */
  justify-content: center; /* ✅ 항상 가운데 정렬 */
  gap: 20px;
  margin-top: 10px;
}

/* ✅ 테두리보다 살짝 여유 있게 — 아이템이 안 짤리도록 */
.chart-item {
  text-align: center;
  width: 80px;
  position: relative;
  box-sizing: border-box; /* ✅ 패딩 포함해서 테두리 넘치지 않게 */
}

.circular-chart {
  display: block;
  margin: auto;
  max-width: 80px;
  transition: transform 0.15s ease;
}

.circular-chart:hover {
  animation: bounceScale 0.6s cubic-bezier(.28,.84,.42,1.2) forwards;
}

.circular-chart.clickable {
  cursor: pointer;
}

.circular-chart.clickable:active {
  transform: scale(0.95);
}

.circular-chart:hover .circle {
  stroke: var(--chart-hover-color);
}

.circular-chart:hover .percentage {
  opacity: 0;
}

.circular-chart:hover .ratio {
  opacity: 1;
}

@keyframes bounceScale {
  0%   { transform: scale(1); }
  40%  { transform: scale(1.15); }
  55%  { transform: scale(1.10); }
  70%  { transform: scale(1.13); }
  85%  { transform: scale(1.11); }
  100% { transform: scale(1.12); }
}

.circle-bg {
  fill: none;
  stroke: #dddfe0;
  stroke-width: 4;
}

.circle {
  fill: none;
  stroke: var(--chart-color);
  stroke-width: 4;
  stroke-linecap: round;
  stroke-dasharray: 0 100;
  animation: fillCircle 1.6s ease forwards;
  transition: stroke 0.3s ease;
}

@keyframes fillCircle {
  from { stroke-dasharray: 0, 100; }
  to { stroke-dasharray: var(--percent), 100; }
}

.percentage,
.ratio {
  font-size: 6px;
  text-anchor: middle;
  dominant-baseline: middle;
  font-weight: bold;
  pointer-events: none;
  transition: opacity 0.3s ease;
  fill: var(--text-color);
}

.ratio {
  opacity: 0;
}

.chart-title {
  margin-top: 4px;
  font-size: 14px;
  font-weight: bold;
}

/* ✅ 화면이 줄어들면 2줄(4+3)로 전환 */
@media (max-width: 1200px) {
  .chart-container {
    grid-template-columns: repeat(4, 80px);
    justify-content: center; /* 두 번째 줄도 가운데 정렬 */
  }
}

/* ✅ 추가적으로 폭이 더 줄어도 4+3 유지 */
@media (max-width: 900px) {
  .chart-container {
    grid-template-columns: repeat(4, 70px);
    gap: 16px;
    justify-content: center;
  }
}
</style>
