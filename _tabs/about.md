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

{% assign badge_url = "https://static.solved.ac/class/c2g.svg" %}

<a href="https://solved.ac/class" class="badge-link">
  <img src="{{ badge_url }}" alt="현재 진행 배지" class="badge-img" loading="lazy" referrerpolicy="no-referrer">
</a>

{% assign classes = "1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list = "16,22,11,0,0,0,0" | split: "," %}
{% assign total_list = "16,22,40,47,48,48,48" | split: "," %}
{% assign colors = "#249CE5,#20C5DF,#1BDF8B,#2BD521,#B0DB15,#EBCA0F,#F3B312" | split: "," %}
{% assign hover_colors = "#5AB4F5,#4DE1E7,#3CEB9D,#54F75D,#D2E635,#F3D425,#F7C73A" | split: "," %}

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
          <path class="circle-bg" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"/>
          <path class="circle" stroke-dasharray="{{ percent | round: 1 }}, 100"
                d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"/>
          <text x="18" y="18" class="percentage">{{ percent | round: 1 }}%</text>
          <text x="18" y="18" class="ratio">{{ solved }}/{{ total }}</text>
        </svg>
      <div class="chart-title">CLASS {{ classes[i] }}</div>
    </div>
  {% endfor %}
</div>

<style>
:root { --chart-size:80px; --gap-x:20px; --gap-y:18px; }
@media (max-width:600px){ :root { --chart-size:70px; --gap-x:16px; --gap-y:16px; } }
@media (max-width:400px){ :root { --chart-size:60px; --gap-x:14px; --gap-y:14px; } }

/* 배지 */
.badge-link{ display:block; text-align:center; margin:0 auto 14px; }
.badge-img{ max-width:180px; height:auto; line-height:0; display:inline-block; }

/* 컨테이너(보더/정렬/랩핑) */
.chart-container{
  display:flex; flex-wrap:nowrap; justify-content:center; align-items:flex-start;
  gap: var(--gap-y) var(--gap-x);
  padding:20px 22px;
  border:1px solid #e5e7eb; border-radius:14px;
  box-sizing:border-box; overflow:hidden; margin-top:6px;
}
@media (prefers-color-scheme:dark){ .chart-container{ border-color:#3a3f45; } }

/* 4칸×2줄(가운데 정렬, 최대 2줄 느낌) */
@media (max-width:1200px){
  .chart-container{
    flex-wrap:wrap;
    width: calc((var(--chart-size)*4) + (var(--gap-x)*3));
    max-width:100%;
    margin-left:auto; margin-right:auto;
    justify-content:center; align-content:center;
  }
}
/* 3칸×2~3줄 */
@media (max-width:700px){
  .badge-img{ max-width:160px; }
  .chart-container{ width: calc((var(--chart-size)*3) + (var(--gap-x)*2)); }
}

/* 차트 요소들은 이전과 동일 */
.chart-item{ text-align:center; width:var(--chart-size); position:relative; }
.circular-chart{ display:block; margin:auto; max-width:var(--chart-size); transition:transform .15s ease; }
.circular-chart:hover{ animation:bounceScale .6s cubic-bezier(.28,.84,.42,1.2) forwards; }
.circular-chart.clickable{ cursor:pointer; }
.circular-chart.clickable:active{ transform:scale(.95); }
.circular-chart:hover .circle{ stroke:var(--chart-hover-color); }
.circular-chart:hover .percentage{ opacity:0; }
.circular-chart:hover .ratio{ opacity:1; }

@keyframes bounceScale{ 0%{transform:scale(1)} 40%{transform:scale(1.15)} 55%{transform:scale(1.10)} 70%{transform:scale(1.13)} 85%{transform:scale(1.11)} 100%{transform:scale(1.12)} }
.circle-bg{ fill:none; stroke:#dddfe0; stroke-width:4; }
.circle{ fill:none; stroke:var(--chart-color); stroke-width:4; stroke-linecap:round; stroke-dasharray:0 100; animation:fillCircle 1.6s ease forwards; transition:stroke .3s ease; }
@keyframes fillCircle{ from{stroke-dasharray:0,100} to{stroke-dasharray:var(--percent),100} }
.percentage,.ratio{ font-size:6px; text-anchor:middle; dominant-baseline:middle; font-weight:bold; pointer-events:none; transition:opacity .3s ease; fill:var(--text-color); }
.ratio{ opacity:0; }
.chart-title{ margin-top:4px; font-size:14px; font-weight:bold; }
</style>
