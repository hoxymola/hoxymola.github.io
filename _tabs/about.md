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

{% assign classes =
"1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list =
"16,20,18,14,12,10,8" | split: "," %}
{% assign total_list =
"20,25,25,20,15,12,10" | split: "," %}
{% assign colors =
"#4CC790,#FFBB33,#FF4444,#33B5E5,#AA66CC,#99CC00,#2BBBAD" | split: "," %}

<div class="chart-container">
  {% for i in (0..6) %}
    {% assign solved = solved_list[i] | plus: 0 %}
    {% assign total = total_list[i] | plus: 0 %}
    {% assign percent = solved | times: 100 | divided_by: total %}
    <div class="chart-item" style="--chart-color: {{ colors[i] }}">
      <svg viewBox="0 0 36 36" class="circular-chart">
        <path class="circle-bg"
              d="M18 2.0845
                 a 15.9155 15.9155 0 0 1 0 31.831
                 a 15.9155 15.9155 0 0 1 0 -31.831"/>
        <path class="circle"
              stroke-dasharray="{{ percent }}, 100"
              d="M18 2.0845
                 a 15.9155 15.9155 0 0 1 0 31.831
                 a 15.9155 15.9155 0 0 1 0 -31.831"/>
        <text x="18" y="20.35" class="percentage">
          {{ percent | round: 1 }}%
        </text>
        <text x="18" y="20.35" class="ratio">
          {{ solved }}/{{ total }}
        </text>
      </svg>
      <div class="chart-title">class {{ classes[i] }}</div>
    </div>
  {% endfor %}
</div>

<style>
.chart-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 18px;
  margin-top: 10px;
}
.chart-item {
  text-align: center;
  width: 120px;
  position: relative;
}
.circular-chart {
  display: block;
  margin: auto;
  max-width: 120px;
  transform: rotate(-90deg);
}
.circle-bg {
  fill: none;
  stroke: #eee;
  stroke-width: 3;
}
.circle {
  fill: none;
  stroke: var(--chart-color);
  stroke-width: 3;
  stroke-linecap: round;
  transition: stroke-dasharray 0.6s ease;
}
.percentage,
.ratio {
  fill: #333;
  font-size: 6px;
  text-anchor: middle;
  transform: rotate(90deg);
  transform-origin: center;
  dominant-baseline: middle;
  font-weight: bold;
  pointer-events: none;
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
  font-weight: 500;
  color: #555;
}
</style>
