<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>미분계수 시각화</title>

<style>

* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #ffffff;
    color: #222222;
    font-family:
        Arial,
        "Noto Sans KR",
        "Malgun Gothic",
        sans-serif;
}

.app {
    width: 100%;
    max-width: 1200px;
    margin: auto;
    padding: 24px;
}

.header {
    text-align: center;
    margin-bottom: 18px;
}

.header h1 {
    margin: 0;
    font-size: 30px;
    font-weight: 700;
}

.header p {
    margin-top: 8px;
    color: #666666;
    font-size: 16px;
}

.main {
    display: grid;
    grid-template-columns: 1fr 300px;
    gap: 20px;
}

.graph-card {
    border: 1px solid #dddddd;
    border-radius: 16px;
    padding: 16px;
    background: white;
}

canvas {
    width: 100%;
    height: auto;
    display: block;
    background: white;
    border-radius: 10px;
}

.panel {
    border: 1px solid #dddddd;
    border-radius: 16px;
    padding: 20px;
    background: #fafafa;
}

.control {
    margin-bottom: 22px;
}

.control-title {
    display: flex;
    justify-content: space-between;
    margin-bottom: 8px;
    font-weight: 600;
}

.value {
    font-family: monospace;
}

input[type="range"] {
    width: 100%;
}

button {
    width: 100%;
    padding: 12px;
    margin-top: 8px;
    border: none;
    border-radius: 10px;
    background: #222222;
    color: white;
    font-size: 15px;
    cursor: pointer;
}

button:hover {
    opacity: 0.85;
}

.info {
    margin-top: 18px;
    padding: 15px;
    background: white;
    border-radius: 12px;
    border: 1px solid #dddddd;
    line-height: 1.8;
}

.formula {
    font-family:
        "Cambria Math",
        "Times New Roman",
        serif;
    font-size: 18px;
}

.highlight {
    font-weight: 700;
}

.legend {
    display: flex;
    gap: 15px;
    flex-wrap: wrap;
    margin-top: 12px;
    font-size: 13px;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
}

.line {
    width: 25px;
    height: 3px;
}

.function-line {
    background: #222222;
}

.tangent-line {
    background: #e53935;
}

.secant-line {
    background: #1976d2;
}

.footer {
    margin-top: 15px;
    padding: 14px;
    border-radius: 12px;
    background: #f3f5f7;
    text-align: center;
    font-size: 14px;
}

@media(max-width: 850px) {

    .main {
        grid-template-columns: 1fr;
    }

    .panel {
        order: -1;
    }

}

</style>
</head>

<body>

<div class="app">

    <div class="header">

        <h1>미분계수의 시각적 이해</h1>

        <p>
            할선이 접선에 가까워지는 과정을 직접 관찰해 봅시다.
        </p>

    </div>


    <div class="main">

        <!-- ========================= -->
        <!-- 그래프 -->
        <!-- ========================= -->

        <div class="graph-card">

            <canvas id="graph"
                    width="800"
                    height="600">
            </canvas>

            <div class="legend">

                <div class="legend-item">
                    <span class="line function-line"></span>
                    f(x) = x²
                </div>

                <div class="legend-item">
                    <span class="line secant-line"></span>
                    할선
                </div>

                <div class="legend-item">
                    <span class="line tangent-line"></span>
                    접선
                </div>

            </div>

        </div>


        <!-- ========================= -->
        <!-- 조작 패널 -->
        <!-- ========================= -->

        <div class="panel">

            <div class="control">

                <div class="control-title">

                    <span>접점 a</span>

                    <span class="value"
                          id="aValue">
                        1.00
                    </span>

                </div>

                <input
                    id="aSlider"
                    type="range"
                    min="-2.5"
                    max="2.5"
                    step="0.01"
                    value="1"
                >

            </div>


            <div class="control">

                <div class="control-title">

                    <span>h</span>

                    <span class="value"
                          id="hValue">
                        1.000
                    </span>

                </div>

                <input
                    id="hSlider"
                    type="range"
                    min="0.001"
                    max="2"
                    step="0.001"
                    value="1"
                >

            </div>


            <button id="animateButton">
                ▶ h → 0
            </button>


            <button id="resetButton">
                ↺ 초기화
            </button>


            <div class="info">

                <div>
                    <span class="highlight">
                        점 A
                    </span>

                    <span id="pointA">
                        (1.00, 1.00)
                    </span>
                </div>


                <div>
                    <span class="highlight">
                        점 B
                    </span>

                    <span id="pointB">
                        (2.00, 4.00)
                    </span>
                </div>

                <hr>


                <div>

                    평균변화율

                    <div class="formula">
                        (f(a+h) − f(a)) / h
                    </div>

                    <span id="secantSlope">
                        3.000
                    </span>

                </div>


                <div style="margin-top:12px;">

                    미분계수

                    <div class="formula">
                        f′(a)
                    </div>

                    <span id="tangentSlope">
                        2.000
                    </span>

                </div>


                <div style="margin-top:12px;">

                    두 기울기의 차이

                    <br>

                    <span id="difference">
                        1.000
                    </span>

                </div>

            </div>

        </div>

    </div>


    <div class="footer">

        <b>관찰 질문</b>
       　
        h가 0에 가까워질수록 할선은 어떻게 변하는가?

        <br>

        평균변화율은 어떤 값에 가까워지는가?

    </div>

</div>


<script>

/* ==========================================================
   기본 설정
========================================================== */

const canvas =
    document.getElementById("graph");

const ctx =
    canvas.getContext("2d");


const aSlider =
    document.getElementById("aSlider");

const hSlider =
    document.getElementById("hSlider");


const aValue =
    document.getElementById("aValue");

const hValue =
    document.getElementById("hValue");


const pointA =
    document.getElementById("pointA");

const pointB =
    document.getElementById("pointB");


const secantSlope =
    document.getElementById("secantSlope");

const tangentSlope =
    document.getElementById("tangentSlope");


const difference =
    document.getElementById("difference");


const animateButton =
    document.getElementById("animateButton");


const resetButton =
    document.getElementById("resetButton");


/* ==========================================================
   좌표계
========================================================== */

const X_MIN = -4;
const X_MAX = 4;

const Y_MIN = -1;
const Y_MAX = 16;


/* ==========================================================
   수학 함수
========================================================== */

function f(x) {

    return x * x;

}


function df(x) {

    return 2 * x;

}


/* ==========================================================
   수학 좌표 → 화면 좌표
========================================================== */

function screenX(x) {

    return (
        (x - X_MIN) /
        (X_MAX - X_MIN)
    ) * canvas.width;

}


function screenY(y) {

    return canvas.height -
        (
            (y - Y_MIN) /
            (Y_MAX - Y_MIN)
        ) * canvas.height;

}


/* ==========================================================
   격자
========================================================== */

function drawGrid() {

    ctx.save();

    ctx.strokeStyle = "#eeeeee";

    ctx.lineWidth = 1;


    for (
        let x = Math.ceil(X_MIN);
        x <= X_MAX;
        x++
    ) {

        const sx = screenX(x);

        ctx.beginPath();

        ctx.moveTo(sx, 0);

        ctx.lineTo(
            sx,
            canvas.height
        );

        ctx.stroke();

    }


    for (
        let y = Math.ceil(Y_MIN);
        y <= Y_MAX;
        y++
    ) {

        const sy = screenY(y);

        ctx.beginPath();

        ctx.moveTo(0, sy);

        ctx.lineTo(
            canvas.width,
            sy
        );

        ctx.stroke();

    }

    ctx.restore();

}


/* ==========================================================
   축
========================================================== */

function drawAxes() {

    ctx.save();

    ctx.strokeStyle = "#555555";

    ctx.lineWidth = 2;


    /* x축 */

    const xAxisY =
        screenY(0);

    ctx.beginPath();

    ctx.moveTo(
        0,
        xAxisY
    );

    ctx.lineTo(
        canvas.width,
        xAxisY
    );

    ctx.stroke();


    /* y축 */

    const yAxisX =
        screenX(0);

    ctx.beginPath();

    ctx.moveTo(
        yAxisX,
        0
    );

    ctx.lineTo(
        yAxisX,
        canvas.height
    );

    ctx.stroke();


    /* 눈금 */

    ctx.fillStyle = "#555555";

    ctx.font =
        "13px Arial";


    for (
        let x = Math.ceil(X_MIN);
        x <= X_MAX;
        x++
    ) {

        if (x === 0) continue;

        const sx =
            screenX(x);

        ctx.beginPath();

        ctx.moveTo(
            sx,
            xAxisY - 4
        );

        ctx.lineTo(
            sx,
            xAxisY + 4
        );

        ctx.stroke();

        ctx.fillText(
            x,
            sx - 4,
            xAxisY + 18
        );

    }


    for (
        let y = Math.ceil(Y_MIN);
        y <= Y_MAX;
        y++
    ) {

        if (y === 0) continue;

        const sy =
            screenY(y);

        ctx.beginPath();

        ctx.moveTo(
            yAxisX - 4,
            sy
        );

        ctx.lineTo(
            yAxisX + 4,
            sy
        );

        ctx.stroke();

        ctx.fillText(
            y,
            yAxisX + 8,
            sy + 4
        );

    }


    ctx.restore();

}


/* ==========================================================
   함수 그래프
========================================================== */

function drawFunction() {

    ctx.save();

    ctx.strokeStyle =
        "#222222";

    ctx.lineWidth = 4;

    ctx.beginPath();


    for (
        let px = 0;
        px <= canvas.width;
        px++
    ) {

        const x =
            X_MIN +
            px / canvas.width *
            (X_MAX - X_MIN);

        const y =
            f(x);

        const sy =
            screenY(y);


        if (px === 0) {

            ctx.moveTo(
                px,
                sy
            );

        } else {

            ctx.lineTo(
                px,
                sy
            );

        }

    }

    ctx.stroke();

    ctx.restore();

}


/* ==========================================================
   직선 그리기
========================================================== */

function drawLine(
    slope,
    intercept,
    color,
    width,
    dash = []
) {

    ctx.save();

    ctx.strokeStyle = color;

    ctx.lineWidth = width;

    ctx.setLineDash(dash);


    const x1 = X_MIN;
    const x2 = X_MAX;

    const y1 =
        slope * x1 +
        intercept;

    const y2 =
        slope * x2 +
        intercept;


    ctx.beginPath();

    ctx.moveTo(
        screenX(x1),
        screenY(y1)
    );

    ctx.lineTo(
        screenX(x2),
        screenY(y2)
    );

    ctx.stroke();

    ctx.restore();

}


/* ==========================================================
   점
========================================================== */

function drawPoint(
    x,
    y,
    color,
    label
) {

    const sx =
        screenX(x);

    const sy =
        screenY(y);


    ctx.save();


    ctx.fillStyle = color;

    ctx.beginPath();

    ctx.arc(
        sx,
        sy,
        8,
        0,
        Math.PI * 2
    );

    ctx.fill();


    ctx.fillStyle =
        "#222222";

    ctx.font =
        "bold 16px Arial";


    ctx.fillText(
        label,
        sx + 10,
        sy - 10
    );


    ctx.restore();

}


/* ==========================================================
   Δx, Δy 보조선
========================================================== */

function drawConstructionLines(
    a,
    h
) {

    const x1 = a;
    const x2 = a + h;

    const y1 = f(x1);
    const y2 = f(x2);


    ctx.save();

    ctx.strokeStyle =
        "#888888";

    ctx.lineWidth = 2;

    ctx.setLineDash([5, 5]);


    /* Δx */

    ctx.beginPath();

    ctx.moveTo(
        screenX(x1),
        screenY(y1)
    );

    ctx.lineTo(
        screenX(x2),
        screenY(y1)
    );

    ctx.stroke();


    /* Δy */

    ctx.beginPath();

    ctx.moveTo(
        screenX(x2),
        screenY(y1)
    );

    ctx.lineTo(
        screenX(x2),
        screenY(y2)
    );

    ctx.stroke();


    ctx.restore();


    /* Δx 표시 */

    ctx.save();

    ctx.fillStyle =
        "#666666";

    ctx.font =
        "14px Arial";


    ctx.fillText(
        "Δx = h",
        (
            screenX(x1) +
            screenX(x2)
        ) / 2 - 25,
        screenY(y1) + 20
    );


    ctx.fillText(
        "Δy",
        screenX(x2) + 8,
        (
            screenY(y1) +
            screenY(y2)
        ) / 2
    );

    ctx.restore();

}


/* ==========================================================
   그래프 전체 그리기
========================================================== */

function drawGraph() {

    const a =
        parseFloat(aSlider.value);

    const h =
        parseFloat(hSlider.value);


    const xA = a;
    const yA = f(a);

    const xB = a + h;
    const yB = f(xB);


    const secant =
        (yB - yA) /
        (xB - xA);


    const tangent =
        df(a);


    const secantIntercept =
        yA -
        secant * xA;


    const tangentIntercept =
        yA -
        tangent * xA;


    /* 화면 초기화 */

    ctx.clearRect(
        0,
        0,
        canvas.width,
        canvas.height
    );


    /* 배경 */

    ctx.fillStyle =
        "#ffffff";

    ctx.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    );


    drawGrid();

    drawAxes();

    drawFunction();


    /* 할선 */

    drawLine(
        secant,
        secantIntercept,
        "#1976d2",
        3,
        [8, 5]
    );


    /* 접선 */

    drawLine(
        tangent,
        tangentIntercept,
        "#e53935",
        3,
        [12, 6]
    );


    drawConstructionLines(
        a,
        h
    );


    drawPoint(
        xA,
        yA,
        "#222222",
        "A"
    );


    drawPoint(
        xB,
        yB,
        "#1976d2",
        "B"
    );


    updateInformation(
        a,
        h,
        secant,
        tangent
    );

}


/* ==========================================================
   정보 업데이트
========================================================== */

function updateInformation(
    a,
    h,
    secant,
    tangent
) {

    const yA = f(a);

    const xB = a + h;

    const yB = f(xB);


    aValue.textContent =
        a.toFixed(2);

    hValue.textContent =
        h.toFixed(3);


    pointA.textContent =
        `(${a.toFixed(2)}, ${yA.toFixed(2)})`;


    pointB.textContent =
        `(${xB.toFixed(2)}, ${yB.toFixed(2)})`;


    secantSlope.textContent =
        secant.toFixed(6);


    tangentSlope.textContent =
        tangent.toFixed(6);


    difference.textContent =
        Math.abs(
            secant - tangent
        ).toFixed(6);

}


/* ==========================================================
   슬라이더
========================================================== */

aSlider.addEventListener(
    "input",
    drawGraph
);


hSlider.addEventListener(
    "input",
    drawGraph
);


/* ==========================================================
   초기화
========================================================== */

resetButton.addEventListener(
    "click",
    function() {

        aSlider.value = 1;

        hSlider.value = 1;

        drawGraph();

    }
);


/* ==========================================================
   애니메이션
========================================================== */

let animating = false;


animateButton.addEventListener(
    "click",
    function() {

        if (animating) return;


        animating = true;

        animateButton.textContent =
            "■ 실행 중";


        const start =
            1.0;

        const end =
            0.001;


        const duration =
            3500;


        const startTime =
            performance.now();


        function animate(
            currentTime
        ) {

            const elapsed =
                currentTime -
                startTime;


            let progress =
                elapsed / duration;


            if (progress > 1) {

                progress = 1;

            }


            /*
                선형 보간 대신
                로그 스케일을 사용하여
                h가 자연스럽게 감소하도록 함
            */

            const logStart =
                Math.log10(start);

            const logEnd =
                Math.log10(end);


            const logH =
                logStart +
                (
                    logEnd -
                    logStart
                ) * progress;


            const h =
                Math.pow(
                    10,
                    logH
                );


            hSlider.value =
                h;


            drawGraph();


            if (progress < 1) {

                requestAnimationFrame(
                    animate
                );

            } else {

                animating = false;

                animateButton.textContent =
                    "▶ h → 0";

            }

        }


        requestAnimationFrame(
            animate
        );

    }
);


/* ==========================================================
   최초 실행
========================================================== */

drawGraph();

</script>

</body>
</html>