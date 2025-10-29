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
.progress-wrap{
  display:flex; flex-direction:column; align-items:center;
  gap:20px; margin-top:24px;
}

.badge-box{
  position: relative;
  isolation: isolate;
  z-index: 10;
}
.badge-box a{ display:inline-block; }
.badge-box img{
  width:180px; height:auto; display:block;
  pointer-events:auto !important; cursor:pointer;
}

.charts{
  border:1px solid var(--text-color);
  border-radius:16px;
  padding:16px 18px;
  position: relative;
  z-index: 0; 
  overflow: visible;
}

.chart-container{
  --item-w: 92px;
  --gap: 20px;
  display: grid;
  grid-template-columns: repeat(7, var(--item-w)); /* 기본: 7개 한 줄 */
  gap: var(--gap);
  justify-content: center;   /* 마지막 줄 포함 항상 가운데 */
}

/* 카드 폭 고정 → 줄이 나뉘어도 간격 유지 */
.chart-item{ width: var(--item-w); padding:10px 6px; border-radius:12px; text-align:center; }

.circular-chart{ 
  display:block; margin:auto;
  max-width: calc(var(--item-w) - 12px);
  transition: transform .15s ease;
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

/* 더 일찍 2줄(4+3)로 전환 — 값은 환경 따라 미세조정 가능 */
@media (max-width: 1280px){
  .chart-container{
    grid-template-columns: repeat(4, var(--item-w)); /* 4열 고정 */
  }
}

/* 더 좁아져도 3열 금지 — 아이템/간격만 줄여서 계속 4열 유지 */
@media (max-width: 900px){
  .chart-container{ --item-w: 84px; --gap:16px; grid-template-columns: repeat(4, var(--item-w)); }
  .circular-chart{ max-width: calc(var(--item-w) - 10px); }
  .chart-title{ font-size: 12px; }
}
@media (max-width: 760px){
  .chart-container{ --item-w: 76px; --gap:14px; grid-template-columns: repeat(4, var(--item-w)); }
}
@media (max-width: 640px){
  .chart-container{ --item-w: 70px; --gap:12px; grid-template-columns: repeat(4, var(--item-w)); }
  .circular-chart{ max-width: calc(var(--item-w) - 8px); }
  .chart-title{ font-size: 11px; }
}
</style>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const badgeLink = document.querySelector('.badge-box a[href="https://solved.ac/class"]');
  const img = badgeLink?.querySelector('img');

  if (!img || !badgeLink) return;

  // 1) 테마가 붙인 이미지 팝업 래퍼를 발견하면 제거(unwrap)하고 다시 우리가 원하는 <a> 안에 넣기
  const popupWrapper = img.closest('a.img-link');
  if (popupWrapper && popupWrapper !== badgeLink) {
    popupWrapper.replaceWith(img);      // 감싼 <a.img-link> 제거
    badgeLink.appendChild(img);         // 원래 배지 링크로 복구
  }

  // 2) 혹시 스크립트가 다시 건드리지 않도록, 식별용 속성 부여(무해)
  img.setAttribute('data-no-image-popup', 'true');

  // 3) 클릭 시 이벤트 전파로 간섭 방지
  badgeLink.addEventListener('click', (e) => { e.stopPropagation(); });
});
</script>
