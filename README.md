#<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>实验室管理系统</title>
    <style>
        /* 基础样式 */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', sans-serif;
        }

        body {
            background: #f0f2f5;
        }

        /* 主页样式 */
        .container {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            padding: 2rem;
            background: #2c3e50;
            color: white;
            border-radius: 10px;
            margin-bottom: 2rem;
        }

        .nav-cards {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 2rem;
        }

        .nav-card {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            transition: transform 0.3s;
            box-shadow: 0 2px 15px rgba(0,0,0,0.1);
        }

        .nav-card:hover {
            transform: translateY(-5px);
        }

        /* 3D容器 */
        #3d-container {
            width: 100%;
            height: 400px;
            border: 1px solid #ddd;
            margin: 1rem 0;
        }

        /* 计算器样式 */
        .calculator {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            max-width: 500px;
            margin: 1rem auto;
        }

        input, button {
            padding: 0.5rem;
            margin: 0.5rem;
            width: 100%;
        }

        /* 切换动画 */
        .content-section {
            display: none;
            opacity: 0;
            transition: opacity 0.5s;
        }

        .active-section {
            display: block;
            opacity: 1;
        }
    </style>
    <!-- Three.js 库 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>
    <div class="container">
        <!-- 主页 -->
        <div id="home" class="active-section">
            <div class="header">
                <h1>实验室管理系统</h1>
                <p>请选择需要使用的功能模块</p>
            </div>
            <div class="nav-cards">
                <div class="nav-card" onclick="showSection('management')">
                    <h2>药品管理系统</h2>
                    <p>试剂定位｜库存管理｜3D导览</p>
                </div>
                <div class="nav-card" onclick="showSection('calculator')">
                    <h2>药品配置工具</h2>
                    <p>浓度计算｜质量换算｜快速定位</p>
                </div>
            </div>
        </div>

        <!-- 药品管理 -->
        <div id="management" class="content-section">
            <button onclick="showSection('home')">返回主页</button>
            <h2>药品管理系统</h2>
            <input type="text" placeholder="搜索药品..." id="searchInput">
            <div id="3d-container"></div>
            <div id="searchResults"></div>
        </div>

        <!-- 配置工具 -->
        <div id="calculator" class="content-section">
            <button onclick="showSection('home')">返回主页</button>
            <h2>溶液配置计算器</h2>
            <div class="calculator">
                <input type="text" placeholder="目标浓度 (mol/L)" id="concentration">
                <input type="text" placeholder="目标体积 (L)" id="volume">
                <button onclick="calculateMass()">计算质量</button>
                <div id="result"></div>
            </div>
        </div>
    </div>

    <script>
        // 页面切换逻辑
        function showSection(sectionId) {
            document.querySelectorAll('.content-section').forEach(sec => {
                sec.classList.remove('active-section');
            });
            document.getElementById(sectionId).classList.add('active-section');
        }

        // 示例3D场景初始化
        function init3DView() {
            const container = document.getElementById('3d-container');
            const scene = new THREE.Scene();
            const camera = new THREE.PerspectiveCamera(75, container.clientWidth/container.clientHeight, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer();
            
            renderer.setSize(container.clientWidth, container.clientHeight);
            container.appendChild(renderer.domElement);

            // 添加示例柜体
            const geometry = new THREE.BoxGeometry();
            const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
            const cube = new THREE.Mesh(geometry, material);
            scene.add(cube);

            camera.position.z = 5;

            function animate() {
                requestAnimationFrame(animate);
                cube.rotation.x += 0.01;
                cube.rotation.y += 0.01;
                renderer.render(scene, camera);
            }
            动画();
        }

        // 计算器逻辑
        function calculateMass() {
            const concentration = parseFloat(document.getElementById('concentration').value);
            const volume = parseFloat(document.getElementById('volume').value);
            const molarMass = 58.44; // 示例：NaCl的摩尔质量
            
            if (!isNaN(浓度) && !isNaN(体积)) {
                const mass = concentration * volume * molarMass;
                document.getElementById('result').innerHTML = `
                    需要称取：${mass.toFixed(2)} g
                    <button onclick="searchChemical('NaCl')">定位药品</button>
                `;
            }
        }

        // 药品搜索功能
        function searchChemical(name) {
            // 此处应连接数据库，以下是示例数据
            const chemicals = {
                'NaCl': {
                    location: 'A-3柜',
                    quantity: '500g',
                    container: '危化柜'
                }
            };

            const result = chemicals[name];
            if (result) {
                document.getElementById('searchResults').innerHTML = `
                    <h3>${name}</h3>
                    <p>位置：${result.location}</p>
                    <p>剩余量：${result.quantity}</p>
                    <p>储存柜：${result.container}</p>
                `;
                showSection('management');
            }
        }

        // 初始化3D视图
        window.onload = init3DView;
    </script>
</body>
</html> 20243031100321
