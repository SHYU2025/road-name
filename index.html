<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>마인크래프트 도시 서버 - 전체 지도 도로 스캐너</title>
    <style>
        body { font-family: 'Malgun Gothic', sans-serif; background-color: #f4f7f6; padding: 20px; color: #333; margin: 0; }
        .wrap { display: flex; gap: 20px; height: calc(100vh - 40px); }
        
        /* 왼쪽 제어 패널 */
        .sidebar { background: #fff; padding: 20px; border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); width: 350px; display: flex; flex-direction: column; overflow-y: auto; }
        h2 { font-size: 1.2rem; margin-top: 0; color: #2c3e50; border-bottom: 2px solid #eee; padding-bottom: 10px; }
        label { font-weight: bold; display: block; margin-top: 15px; margin-bottom: 5px; font-size: 0.95rem; }
        .hint { font-size: 0.8rem; color: #7f8c8d; margin-bottom: 8px; display: block; }
        input, textarea, button { width: 100%; box-sizing: border-box; padding: 10px; border: 1px solid #ddd; border-radius: 5px; margin-bottom: 10px; }
        textarea { height: 100px; font-family: monospace; resize: vertical; }
        .flex-row { display: flex; gap: 10px; }
        button { background-color: #2980b9; color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.2s; }
        button:hover { background-color: #2471a3; }
        button.action-btn { background-color: #27ae60; padding: 15px; font-size: 1.1rem; margin-top: 10px; }
        button.action-btn:hover { background-color: #219150; }
        
        #resultArea { margin-top: 15px; padding: 15px; background: #e8f6f3; border-left: 4px solid #27ae60; display: none; }
        
        /* 오른쪽 지도 패널 (스크롤 가능) */
        .map-area { flex: 1; background: #ecf0f1; border-radius: 10px; border: 2px dashed #bdc3c7; overflow: auto; position: relative; display: flex; align-items: center; justify-content: center; }
        
        /* 캔버스와 이미지를 겹치기 위한 래퍼 (원본 크기 유지) */
        #mapWrapper { position: relative; margin: auto; display: none; cursor: crosshair; }
        #mapImage { display: block; user-select: none; pointer-events: none; }
        #overlayCanvas { position: absolute; top: 0; left: 0; pointer-events: none; }
    </style>
</head>
<body>

<div class="wrap">
    <div class="sidebar">
        <h2>🗺️ 1. 전체 지도 설정 (1px = 1block)</h2>
        <input type="file" id="imageLoader" accept="image/*">
        
        <label>지도 영점 (맨 왼쪽 위 좌표)</label>
        <span class="hint">이미지 가장 좌측 상단의 X, Z 좌표를 입력하세요.</span>
        <div class="flex-row">
            <input type="number" id="startX" placeholder="왼쪽 위 X" value="-2000">
            <input type="number" id="startZ" placeholder="왼쪽 위 Z" value="-2000">
        </div>

        <h2>📍 2. 도로 웨이포인트 스캔</h2>
        <span class="hint">지도를 클릭하여 도로를 스캔하세요.</span>
        <textarea id="roadInput"></textarea>
        <button onclick="clearRoads()" style="background-color: #e74c3c;">도로 좌표 초기화</button>

        <h2>🏠 3. 건물 주소 발급</h2>
        <div class="flex-row">
            <input type="number" id="bX" placeholder="건물 입구 X">
            <input type="number" id="bZ" placeholder="건물 입구 Z">
        </div>
        <button class="action-btn" onclick="calculateAddress()">주소 발급하기</button>

        <div id="resultArea"></div>
    </div>

    <div class="map-area" id="mapScrollArea">
        <span id="placeholderText" style="color: #7f8c8d;">전체 지도를 업로드하세요. (스크롤 가능)</span>
        <div id="mapWrapper">
            <img id="mapImage" src="" alt="Map">
            <canvas id="overlayCanvas"></canvas>
        </div>
    </div>
</div>

<script>
    const imageLoader = document.getElementById('imageLoader');
    const mapWrapper = document.getElementById('mapWrapper');
    const mapImage = document.getElementById('mapImage');
    const overlayCanvas = document.getElementById('overlayCanvas');
    const ctx = overlayCanvas.getContext('2d');
    const roadInput = document.getElementById('roadInput');
    const placeholderText = document.getElementById('placeholderText');
    const mapScrollArea = document.querySelector('.map-area');
    
    let clickedPoints = [];

    // 1. 1:1 이미지 업로드 및 초기화
    imageLoader.addEventListener('change', function(e) {
        const file = e.target.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = function(event) {
            mapImage.src = event.target.result;
            mapImage.onload = () => {
                mapScrollArea.style.alignItems = 'flex-start';
                mapScrollArea.style.justifyContent = 'flex-start';
                placeholderText.style.display = 'none';
                mapWrapper.style.display = 'block';
                
                // 캔버스 크기를 이미지 원본 픽셀 크기와 1:1로 맞춤
                overlayCanvas.width = mapImage.naturalWidth;
                overlayCanvas.height = mapImage.naturalHeight;
                
                clearRoads();
            }
        }
        reader.readAsDataURL(file);
    });

    // 2. 지도 클릭: 1픽셀 = 1블록 직관적 계산
    mapWrapper.addEventListener('click', function(e) {
        // 이미지가 스크롤된 상태에서도 정확한 픽셀 오프셋을 가져옴
        const rect = mapWrapper.getBoundingClientRect();
        const px = Math.round(e.clientX - rect.left);
        const py = Math.round(e.clientY - rect.top);

        const startX = parseFloat(document.getElementById('startX').value) || 0;
        const startZ = parseFloat(document.getElementById('startZ').value) || 0;

        // 1px = 1block 이므로 클릭한 픽셀 수만큼 단순히 더해줌
        const mcX = startX + px;
        const mcZ = startZ + py;
        
        if (roadInput.value.trim() !== "") roadInput.value += "\n";
        roadInput.value += `${mcX}, ${mcZ}`;

        clickedPoints.push({ x: px, y: py });
        drawOverlay();
    });

    function drawOverlay() {
        ctx.clearRect(0, 0, overlayCanvas.width, overlayCanvas.height);
        
        if (clickedPoints.length > 1) {
            ctx.beginPath();
            ctx.lineWidth = 4;
            ctx.strokeStyle = '#3498db';
            ctx.moveTo(clickedPoints[0].x, clickedPoints[0].y);
            for (let i = 1; i < clickedPoints.length; i++) {
                ctx.lineTo(clickedPoints[i].x, clickedPoints[i].y);
            }
            ctx.stroke();
        }

        ctx.fillStyle = '#e74c3c';
        clickedPoints.forEach(p => {
            ctx.beginPath();
            ctx.arc(p.x, p.y, 6, 0, Math.PI * 2);
            ctx.fill();
        });
    }

    function clearRoads() {
        roadInput.value = "";
        clickedPoints = [];
        ctx.clearRect(0, 0, overlayCanvas.width, overlayCanvas.height);
    }

    // 3. 건물 주소 계산 핵심 알고리즘 (이전 로직 동일)
    function calculateAddress() {
        const rawLines = roadInput.value.trim().split('\n');
        const road = [];
        for (let line of rawLines) {
            const parts = line.split(',');
            if (parts.length >= 2) road.push({ x: parseFloat(parts[0]), z: parseFloat(parts[1]) });
        }

        const bX = parseFloat(document.getElementById('bX').value);
        const bZ = parseFloat(document.getElementById('bZ').value);

        if (road.length < 2 || isNaN(bX) || isNaN(bZ)) {
            alert("지도를 클릭해 도로를 그리고, 건물 입구 좌표를 입력하세요!");
            return;
        }

        let minDistance = Infinity;
        let bestSegment = null;
        let currentRoadLength = 0;

        for (let i = 0; i < road.length - 1; i++) {
            const A = road[i], B = road[i + 1];
            const ABx = B.x - A.x, ABz = B.z - A.z;
            const APx = bX - A.x, APz = bZ - A.z;

            const segLenSq = ABx * ABx + ABz * ABz;
            const segLen = Math.sqrt(segLenSq);

            let t = segLenSq > 0 ? (APx * ABx + APz * ABz) / segLenSq : 0;
            t = Math.max(0, Math.min(1, t));

            const projX = A.x + t * ABx, projZ = A.z + t * ABz;
            const dist = Math.sqrt((bX - projX) ** 2 + (bZ - projZ) ** 2);

            if (dist < minDistance) {
                minDistance = dist;
                bestSegment = { A, B, distBefore: currentRoadLength, distOnSegment: t * segLen };
            }
            currentRoadLength += segLen;
        }

        const cross = (bestSegment.B.x - bestSegment.A.x) * (bZ - bestSegment.A.z) - 
                      (bestSegment.B.z - bestSegment.A.z) * (bX - bestSegment.A.x);
        
        const isLeft = cross > 0;
        const totalDistance = bestSegment.distBefore + bestSegment.distOnSegment;
        
        let baseNum = Math.floor(totalDistance / 10) * 2;
        if (baseNum === 0) baseNum = 2;
        const addressNum = isLeft ? baseNum - 1 : baseNum;

        const res = document.getElementById('resultArea');
        res.style.display = 'block';
        res.innerHTML = `건물 번호: <strong style="color:#e74c3c; font-size:1.4em;">${addressNum}</strong><br>
                         (위치: ${isLeft ? "왼쪽" : "오른쪽"}, 거리: ${totalDistance.toFixed(1)}m)`;
    }
</script>

</body>
</html>
