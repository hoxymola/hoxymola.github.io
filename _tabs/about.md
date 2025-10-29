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

{% assign classes = "1,2,3,4,5,6,7" | split: "," %}
{% assign solved_list = "16,22,11,0,0,0,0" | split: "," %}
{% assign total_list = "16,22,40,47,48,48,48" | split: "," %}
{% assign colors = "#249CE5,#20C5DF,#1BDF8B,#2BD521,#B0DB15,#EBCA0F,#F3B312" | split: "," %}
{% assign hover_colors = "#5AB4F5,#4DE1E7,#3CEB9D,#54F75D,#D2E635,#F3D425,#F7C73A" | split: "," %}

<div class="progress-wrapper">
  <!-- 배지 -->
  <div class="badge-box">
    <img src="{{ badge_url }}" alt="현재 진행 배지"
         class="progress-badge"
         loading="lazy" decoding="async" referrerpolicy="no-referrer">
  </div>

  <!-- 차트들 -->
  <div class="charts-box">
    <div class="chart-container">
      {% for i in (0..6) %}
        {% assign solved = solved_list[i] | plus: 0 %}
        {% assign total = total_list[i] | plus: 0 %}
        {% assign percent = solved | times: 100 | divided_by: total %}

        <div class="chart-item"
             style="--chart-color: {{ colors[i] }}; --chart-hover-color: {{ hover_colors[i] }}; --percent: {{ percent }}">
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
  </div>
</div>

<style>
/* ---- 공통 토큰 ---- */
:root { --chart-size: 80px; }
@media (max-width: 600px) { :root { --chart-size: 70px; } }
@media (max-width: 400px) { :root { --chart-size: 60px; } }

/* ---- 래퍼(보더/코너/클리핑) ---- */
.progress-wrapper{
  display:flex; align-items:center; gap:28px;
  padding:20px 22px;
  border:1px solid var(--text-color); border-radius:14px;
  overflow:hidden; /* 보더 넘침 방지 */
  box-sizing:border-box;
}

/* ---- 배지 ---- */
.badge-box{ flex:0 0 auto; display:flex; align-items:center; justify-content:center; }
.progress-badge{
  max-width:180px; height:auto; display:block;
  filter:drop-shadow(0 2px 6px rgba(0,0,0,.06));
}

/* ---- 차트 영역 ---- */
.charts-box{ flex:1 1 auto; }

/* 기본: 넓은 화면에서는 가로 1줄(넘치면 자동으로 2단 그리드로 전환됨) */
.chart-container{
  display:flex; flex-wrap:nowrap; justify-content:center; gap:20px;
}

/* 넘치기 시작하는 구간부터는 Grid로 바꿔 중앙 정렬 + 균일한 여백 */
@media (max-width: 1200px){
  .progress-wrapper{ flex-direction:column; gap:18px; }
  .chart-container{
    display:grid;
    grid-template-columns: repeat(4, minmax(var(--chart-size), 1fr)); /* 4칸 */
    justify-content:center;            /* 전체 그리드 중앙 정렬 */
    place-items:center;                /* 아이템 중앙 정렬 */
    column-gap:20px; row-gap:24px;     /* 가로/세로 간격 고정 */
    width:100%;
  }
}

/* 더 좁으면 3칸 그리드 */
@media (max-width: 700px){
  .progress-badge{ max-width:160px; }
  .chart-container{
    grid-template-columns: repeat(3, minmax(var(--chart-size), 1fr)); /* 3칸 */
    column-gap:16px; row-gap:22px;
  }
}

/* ---- 차트 아이템/스타일 ---- */
.chart-item{ text-align:center; width:var(--chart-size); position:relative; }
.circular-chart{ display:block; margin:auto; max-width:var(--chart-size); transition:transform .15s ease; }
.circular-chart:hover{ animation:bounceScale .6s cubic-bezier(.28,.84,.42,1.2) forwards; }
.circular-chart.clickable{ cursor:pointer; }
.circular-chart.clickable:active{ transform:scale(.95); }
.circular-chart:hover .circle{ stroke:var(--chart-hover-color); }
.circular-chart:hover .percentage{ opacity:0; }
.circular-chart:hover .ratio{ opacity:1; }

@keyframes bounceScale{
  0%{transform:scale(1)} 40%{transform:scale(1.15)}
  55%{transform:scale(1.10)} 70%{transform:scale(1.13)}
  85%{transform:scale(1.11)} 100%{transform:scale(1.12)}
}

.circle-bg{ fill:none; stroke:#dddfe0; stroke-width:4; }
.circle{
  fill:none; stroke:var(--chart-color); stroke-width:4; stroke-linecap:round;
  stroke-dasharray:0 100; animation:fillCircle 1.6s ease forwards; transition:stroke .3s ease;
}
@keyframes fillCircle{ from{stroke-dasharray:0,100} to{stroke-dasharray:var(--percent),100} }

.percentage,.ratio{
  font-size:6px; text-anchor:middle; dominant-baseline:middle;
  font-weight:bold; pointer-events:none; transition:opacity .3s ease; fill:var(--text-color);
}
.ratio{ opacity:0; }
.chart-title{ margin-top:4px; font-size:14px; font-weight:bold; }

/* (옵션) 각 차트에 미세 테두리 주고 싶다면 주석 해제
.chart-item{ border:1px solid rgba(0,0,0,.06); border-radius:10px; padding:8px 6px; }
@media (prefers-color-scheme: dark){ .chart-item{ border-color:rgba(255,255,255,.12); } }
*/
</style>
