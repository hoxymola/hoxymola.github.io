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

[![현재 진행 배지](/assets/class/c2g.svg){: width="180" }](https://solved.ac/class)

{% assign classes = "1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list = "16,22,11,0,0,0,0" | split: "," %}
{% assign total_list = "16,22,40,47,48,48,48" | split: "," %}
{% assign colors = "#249CE5,#20C5DF,#1BDF8B,#2BD521,#B0DB15,#EBCA0F,#F3B312" | split: "," %}
{% assign hover_colors = "#5AB4F5,#4DE1E7,#3CEB9D,#54F75D,#D2E635,#F3D425,#F7C73A" | split: "," %}

<div class="progress-wrap">
  <div class="badge-box">
    <a href="https://solved.ac/class">
      <img src="/assets/class/c2g.svg" alt="현재 진행 배지" width="180"/>
    </a>
  </div>

  <div class="charts">
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
</div>

<style>
/* ---- 레이아웃 래퍼: 배지(좌) + 차트(우) ---- */
.progress-wrap{
  display:flex;
  align-items:center;
  gap:28px;
  margin-top: 24px;
}

/* 큰 화면: 배지는 왼쪽 고정 */
.badge-box img{
  width: 180px;
  height:auto;
  display:block;
}

/* 차트 박스에 외곽선 추가 */
.charts{
  border:1px solid rgba(0,0,0,.08);
  border-radius:16px;
  padding:16px 18px;
}

/* ---- 차트 컨테이너 ---- */
/* 데스크톱: 한 줄(가로 스크롤 없이 배치되면 그대로 1줄, 좁아져서 못 들어가면 아래 미디어쿼리에서 분기) */
.chart-container{
  display:grid;
  grid-auto-flow: column;
  grid-auto-columns: auto;
  gap:20px;
  align-items: center;
}

.chart-item{
  text-align:center;
  width: 92px;
  position:relative;
  padding:10px 6px;
  border-radius:12px;
  background: var(--card-bg, #fff);
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

/* ---- 반응형 분기 ---- */
/* 중간 화면(~1200px): 배지가 위로, 차트는 4열 그리드 + 가운데 정렬 */
@media (max-width: 1200px){
  .progress-wrap{ flex-direction: column; align-items: center; }
  .chart-container{
    grid-auto-flow: row;                /* 열 흐름 해제하고 표준 그리드로 */
    grid-template-columns: repeat(4, auto); /* 4개씩 */
    justify-content: center;            /* 줄 단위 가운데 정렬 */
  }
}

/* 모바일(~768px): 3열 그리드 + 가운데 정렬 */
@media (max-width: 768px){
  .chart-container{
    grid-template-columns: repeat(3, auto); /* 3개씩 */
    justify-content: center;
    gap:16px;
  }
  .chart-item{ width: 88px; }
  .circular-chart{ max-width: 70px; }
  .chart-title{ font-size: 12px; }
}

/* 더 작은 모바일(~480px): 간격/사이즈 살짝 축소 */
@media (max-width: 480px){
  .chart-container{ gap:12px; }
  .chart-item{ width: 80px; padding:8px 6px; }
  .circular-chart{ max-width: 60px; }
  .chart-title{ font-size: 11px; }
}
</style>
