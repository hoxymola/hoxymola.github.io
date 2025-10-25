---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

### 좋아해요 🙂
🐶 강쥐\
🎮 게임\
🍗🍕🍔 치킨피자햄버거\
☔️ 비\
😴 드르렁\
☃️ 겨울

### 싫어해요 🙃
🦟 벌레\
🧑‍💻 일\
🫘🍆🍄‍🟫 콩가지버섯\
☀️ 햇빛\
🚶 뚜벅뚜벅\
🏖️ 여름

### 여기까지 했어요 😎

{% assign classes = "1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list = "16,22,7,0,0,0,0" | split: "," %}
{% assign total_list = "16,22,40,47,48,48,48" | split: "," %}
{% assign colors = "#249CE5,#20C5DF,#1BDF8B,#2BD521,#B0DB15,#EBCA0F,#F3B312" | split: "," %}
{% assign hover_colors = "#5AB4F5,#4DE1E7,#3CEB9D,#54F75D,#D2E635,#F3D425,#F7C73A" | split: "," %}

<div class="chart-container">
  {% for i in (0..6) %}
    {% assign solved = solved_list[i] | plus: 0 %}
    {% assign total = total_list[i] | plus: 0 %}
    {% assign percent = solved | times: 100 | divided_by: total %}
    <div class="chart-item" style="--chart-color: {{ colors[i] }}; --chart-hover-color: {{ hover_colors[i] }}; --percent: {{ percent }}">
      <svg viewBox="0 0 36 36" class="circular-chart">
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

<style>
.chart-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  margin-top: 30px;
}

.chart-item {
  text-align: center;
  flex: 1 1 calc(100% / 4 - 20px); /* 최대 4개씩 한 줄 */
  max-width: 80px;
  position: relative;
  transition: transform 0.4s cubic-bezier(.28,.84,.42,1.2), box-shadow 0.4s ease;
}

.chart-item:hover {
  transform: scale(1.12);
  box-shadow: 0 6px 15px rgba(0,0,0,0.15);
}

.circular-chart {
  display: block;
  margin: auto;
  max-width: 80px;
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

.chart-item:hover .circle {
  stroke: var(--chart-hover-color);
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
}

.ratio {
  opacity: 0;
}

.chart-item:hover .percentage {
  opacity: 0;
}

.chart-item:hover .ratio {
  opacity: 1;
}

.chart-title {
  margin-top: 4px;
  font-size: 14px;
  font-weight: bold;
  transition: transform 0.3s ease;
}

@media (max-width: 1024px) {
  .chart-item { flex: 1 1 calc(100% / 3 - 16px); max-width: 70px; } /* 3개씩 한 줄 */
}

@media (max-width: 768px) {
  .chart-item { flex: 1 1 calc(100% / 3 - 12px); max-width: 70px; }
  .chart-title { font-size: 12px; }
}

@media (max-width: 480px) {
  .chart-item { flex: 1 1 calc(100% / 2 - 10px); max-width: 60px; }
  .circular-chart { max-width: 60px; }
  .chart-title { font-size: 11px; }
}
</style>
