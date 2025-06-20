<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D图片立方体展示器</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 40px 20px;
            color: #fff;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 1200px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            width: 100%;
            animation: fadeIn 1s ease;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            background: linear-gradient(to right, #fff, #a8edea);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
            max-width: 600px;
            margin: 0 auto;
            line-height: 1.6;
        }
        
        .tabs {
            display: flex;
            gap: 15px;
            margin: 30px 0;
        }
        
        .tab {
            background: rgba(255, 255, 255, 0.1);
            padding: 12px 25px;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .tab.active {
            background: #4a6bff;
            box-shadow: 0 5px 15px rgba(74, 107, 255, 0.4);
        }
        
        .tab:hover:not(.active) {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .upload-container {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            width: 100%;
            max-width: 600px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.2);
            margin-bottom: 40px;
            animation: slideUp 0.8s ease;
        }
        
        .upload-area {
            border: 3px dashed rgba(255, 255, 255, 0.4);
            border-radius: 15px;
            padding: 50px 30px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-bottom: 30px;
            position: relative;
        }
        
        .upload-area:hover {
            background: rgba(255, 255, 255, 0.1);
            border-color: rgba(255, 255, 255, 0.7);
        }
        
        .upload-icon {
            font-size: 5rem;
            margin-bottom: 20px;
            color: rgba(255, 255, 255, 0.7);
        }
        
        .upload-text {
            font-size: 1.3rem;
            margin-bottom: 15px;
        }
        
        .file-info {
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        .file-input {
            display: none;
        }
        
        .btn {
            background: #4a6bff;
            color: white;
            border: none;
            padding: 14px 35px;
            font-size: 1.1rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            outline: none;
            margin-top: 10px;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }
        
        .btn i {
            font-size: 1.2rem;
        }
        
        .btn:hover {
            background: #5a7bff;
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        .btn:disabled {
            background: #7187d0;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .cube-section {
            width: 100%;
            max-width: 800px;
            margin: 0 auto 60px;
            display: flex;
            flex-direction: column;
            align-items: center;
            animation: fadeIn 1.2s ease;
        }
        
        .cube-container {
            width: 100%;
            height: 400px;
            perspective: 1200px;
            margin: 20px auto;
            position: relative;
        }
        
        .cube {
            width: 200px;
            height: 200px;
            position: relative;
            transform-style: preserve-3d;
            transform: translateZ(-100px) rotateX(-20deg) rotateY(-20deg);
            transition: transform 0.5s ease;
            margin: 50px auto;
        }
        
        .cube-face {
            position: absolute;
            width: 200px;
            height: 200px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            font-size: 3rem;
            color: rgba(255, 255, 255, 0.2);
            box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.1);
        }
        
        .cube-face img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .acrylic {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.15), rgba(255, 255, 255, 0.05));
            backdrop-filter: blur(10px);
            box-shadow: inset 0 0 30px rgba(255, 255, 255, 0.2);
        }
        
        .front  { transform: rotateY(  0deg) translateZ(100px); }
        .back   { transform: rotateY(180deg) translateZ(100px); }
        .right  { transform: rotateY( 90deg) translateZ(100px); }
        .left   { transform: rotateY(-90deg) translateZ(100px); }
        .top    { transform: rotateX( 90deg) translateZ(100px); }
        .bottom { transform: rotateX(-90deg) translateZ(100px); }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
            width: 100%;
        }
        
        .control-btn {
            background: rgba(255, 255, 255, 0.15);
            color: white;
            border: none;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .control-btn:hover {
            background: rgba(255, 255, 255, 0.25);
            transform: translateY(-3px);
        }
        
        .control-btn:active {
            transform: translateY(0);
        }
        
        .auto-rotate {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.2);
            padding: 12px 20px;
            border-radius: 30px;
            display: flex;
            align-items: center;
            gap: 10px;
            backdrop-filter: blur(5px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
        }
        
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }
        
        .switch input { 
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }
        
        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .slider {
            background-color: #4a6bff;
        }
        
        input:checked + .slider:before {
            transform: translateX(26px);
        }
        
        .preview-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            margin-top: 20px;
            max-width: 600px;
        }
        
        .preview-item {
            width: 80px;
            height: 80px;
            border-radius: 10px;
            overflow: hidden;
            position: relative;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s ease;
        }
        
        .preview-item:hover {
            transform: scale(1.05);
        }
        
        .preview-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .preview-item .remove {
            position: absolute;
            top: 5px;
            right: 5px;
            background: rgba(255, 0, 0, 0.7);
            color: white;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .preview-item:hover .remove {
            opacity: 1;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 100;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }
        
        .modal-content {
            background: linear-gradient(135deg, #2c3e50, #1a2980);
            border-radius: 20px;
            padding: 40px;
            max-width: 500px;
            width: 90%;
            text-align: center;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
            animation: popIn 0.4s ease;
        }
        
        .modal h2 {
            font-size: 1.8rem;
            margin-bottom: 20px;
        }
        
        .modal p {
            font-size: 1.1rem;
            margin-bottom: 30px;
            line-height: 1.6;
        }
        
        .modal-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }
        
        .modal-btn {
            padding: 12px 30px;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
            font-size: 1rem;
        }
        
        .modal-btn.continue {
            background: #4a6bff;
            color: white;
        }
        
        .modal-btn.cancel {
            background: rgba(255, 255, 255, 0.15);
            color: white;
        }
        
        .modal-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 25px;
            margin-top: 30px;
            max-width: 800px;
            width: 100%;
        }
        
        .instructions h3 {
            font-size: 1.4rem;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .steps {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }
        
        .step {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            flex: 1;
            min-width: 200px;
            text-align: center;
        }
        
        .step-number {
            background: #4a6bff;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 15px;
            font-weight: bold;
        }
        
        footer {
            margin-top: 40px;
            text-align: center;
            opacity: 0.7;
            font-size: 0.9rem;
        }
        
        /* 动画 */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { 
                opacity: 0;
                transform: translateY(50px);
            }
            to { 
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes popIn {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        
        @keyframes rotate {
            from { transform: translateZ(-100px) rotateX(-20deg) rotateY(0deg); }
            to { transform: translateZ(-100px) rotateX(-20deg) rotateY(360deg); }
        }
        
        .cube-spin {
            animation: rotate 20s linear infinite;
        }
        
        @media (max-width: 768px) {
            .upload-container {
                padding: 25px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .cube-container {
                height: 350px;
            }
            
            .cube {
                width: 180px;
                height: 180px;
            }
            
            .cube-face {
                width: 180px;
                height: 180px;
            }
            
            .tabs {
                flex-wrap: wrap;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>3D图片立方体展示器</h1>
            <p class="subtitle">上传1-6张图片，创建炫酷的3D立方体展示效果，支持多种图片布局和交互控制</p>
        </header>
        
        <div class="tabs">
            <div class="tab active" id="uploadTab">上传图片</div>
            <div class="tab" id="previewTab">预览立方体</div>
        </div>
        
        <div class="upload-container" id="uploadSection">
            <div class="upload-area" id="uploadArea">
                <div class="upload-icon">📁</div>
                <div class="upload-text">点击或拖拽图片到此处</div>
                <div class="file-info">支持格式: PNG, JPEG, JPG, BMP, GIF (最多6张)</div>
                <input type="file" id="fileInput" class="file-input" accept=".png,.jpeg,.jpg,.bmp,.gif" multiple>
            </div>
            
            <button id="generateBtn" class="btn" disabled>
                <i class="fas fa-cube"></i> 生成立方体
            </button>
            
            <div class="preview-container" id="previewContainer"></div>
        </div>
        
        <div class="cube-section" id="cubeSection" style="display:none;">
            <div class="controls">
                <button class="control-btn" id="rotateLeftBtn" title="逆时针旋转">
                    <i class="fas fa-undo"></i>
                </button>
                <button class="control-btn" id="resetBtn" title="重置位置">
                    <i class="fas fa-sync-alt"></i>
                </button>
                <button class="control-btn" id="rotateRightBtn" title="顺时针旋转">
                    <i class="fas fa-redo"></i>
                </button>
            </div>
            
            <div class="cube-container">
                <div class="cube" id="cube">
                    <div class="cube-face front"><img src="" alt="Front"></div>
                    <div class="cube-face back"><img src="" alt="Back"></div>
                    <div class="cube-face right"><img src="" alt="Right"></div>
                    <div class="cube-face left"><img src="" alt="Left"></div>
                    <div class="cube-face top"><img src="" alt="Top"></div>
                    <div class="cube-face bottom"><img src="" alt="Bottom"></div>
                </div>
                
                <div class="auto-rotate">
                    <span>自动旋转</span>
                    <label class="switch">
                        <input type="checkbox" id="autoRotate" checked>
                        <span class="slider"></span>
                    </label>
                </div>
            </div>
        </div>
        
        <div class="instructions">
            <h3>使用说明</h3>
            <div class="steps">
                <div class="step">
                    <div class="step-number">1</div>
                    <p>上传1-6张图片（支持PNG、JPEG、JPG、BMP、GIF格式）</p>
                </div>
                <div class="step">
                    <div class="step-number">2</div>
                    <p>根据上传图片数量，系统会提示您确认生成方式</p>
                </div>
                <div class="step">
                    <div class="step-number">3</div>
                    <p>点击"生成立方体"按钮创建您的3D图片立方体</p>
                </div>
                <div class="step">
                    <div class="step-number">4</div>
                    <p>使用控制按钮旋转立方体或开启/关闭自动旋转</p>
                </div>
            </div>
        </div>
        
        <footer>
            <p>© 2023 3D图片立方体展示器 | 交互式3D图片展示工具</p>
        </footer>
    </div>
    
    <div class="modal" id="confirmModal">
        <div class="modal-content">
            <h2 id="modalTitle">确认操作</h2>
            <p id="modalMessage">您确定要执行此操作吗？</p>
            <div class="modal-buttons">
                <button class="modal-btn continue" id="confirmContinue">继续生成</button>
                <button class="modal-btn cancel" id="confirmCancel">重新选择图片</button>
            </div>
        </div>
    </div>
    
    <script>
        // 全局变量
        let uploadedImages = [];
        let isDragging = false;
        let startX, startY;
        let rotateX = -20;
        let rotateY = -20;
        let autoRotate = true;
        let animationId = null;
        let autoRotateInterval = null;
        
        // DOM元素
        const uploadArea = document.getElementById('uploadArea');
        const fileInput = document.getElementById('fileInput');
        const generateBtn = document.getElementById('generateBtn');
        const previewContainer = document.getElementById('previewContainer');
        const cube = document.getElementById('cube');
        const autoRotateCheckbox = document.getElementById('autoRotate');
        const confirmModal = document.getElementById('confirmModal');
        const modalTitle = document.getElementById('modalTitle');
        const modalMessage = document.getElementById('modalMessage');
        const confirmContinue = document.getElementById('confirmContinue');
        const confirmCancel = document.getElementById('confirmCancel');
        const cubeFaces = document.querySelectorAll('.cube-face');
        const uploadSection = document.getElementById('uploadSection');
        const cubeSection = document.getElementById('cubeSection');
        const uploadTab = document.getElementById('uploadTab');
        const previewTab = document.getElementById('previewTab');
        const rotateLeftBtn = document.getElementById('rotateLeftBtn');
        const rotateRightBtn = document.getElementById('rotateRightBtn');
        const resetBtn = document.getElementById('resetBtn');
        
        // 初始化事件监听器
        function init() {
            uploadArea.addEventListener('click', () => fileInput.click());
            uploadArea.addEventListener('dragover', handleDragOver);
            uploadArea.addEventListener('drop', handleDrop);
            fileInput.addEventListener('change', handleFileSelect);
            generateBtn.addEventListener('click', handleGenerate);
            autoRotateCheckbox.addEventListener('change', toggleAutoRotate);
            confirmContinue.addEventListener('click', continueGeneration);
            confirmCancel.addEventListener('click', cancelGeneration);
            
            // 立方体旋转控制
            cube.parentElement.addEventListener('mousedown', startDrag);
            document.addEventListener('mousemove', drag);
            document.addEventListener('mouseup', stopDrag);
            
            // 标签切换
            uploadTab.addEventListener('click', () => switchTab('upload'));
            previewTab.addEventListener('click', () => switchTab('preview'));
            
            // 旋转控制按钮
            rotateLeftBtn.addEventListener('click', rotateLeft);
            rotateRightBtn.addEventListener('click', rotateRight);
            resetBtn.addEventListener('click', resetPosition);
            
            // 初始化立方体旋转
            animateCube();
        }
        
        // 标签切换
        function switchTab(tab) {
            if (tab === 'upload') {
                uploadSection.style.display = 'block';
                cubeSection.style.display = 'none';
                uploadTab.classList.add('active');
                previewTab.classList.remove('active');
            } else {
                if (uploadedImages.length > 0) {
                    uploadSection.style.display = 'none';
                    cubeSection.style.display = 'flex';
                    uploadTab.classList.remove('active');
                    previewTab.classList.add('active');
                } else {
                    alert('请先上传图片并生成立方体');
                }
            }
        }
        
        // 处理拖拽事件
        function handleDragOver(e) {
            e.preventDefault();
            e.stopPropagation();
            uploadArea.style.backgroundColor = 'rgba(255, 255, 255, 0.2)';
        }
        
        function handleDrop(e) {
            e.preventDefault();
            e.stopPropagation();
            uploadArea.style.backgroundColor = '';
            
            if (e.dataTransfer.files && e.dataTransfer.files.length > 0) {
                processFiles(e.dataTransfer.files);
            }
        }
        
        // 处理文件选择
        function handleFileSelect(e) {
            if (e.target.files && e.target.files.length > 0) {
                processFiles(e.target.files);
            }
        }
        
        // 处理上传的文件
        function processFiles(files) {
            const validTypes = ['image/png', 'image/jpeg', 'image/jpg', 'image/bmp', 'image/gif'];
            
            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                
                if (!validTypes.includes(file.type)) {
                    alert(`文件 ${file.name} 不是支持的图片格式。请上传PNG、JPEG、JPG、BMP或GIF格式的图片。`);
                    continue;
                }
                
                if (uploadedImages.length >= 6) {
                    alert('最多只能上传6张图片。');
                    break;
                }
                
                const reader = new FileReader();
                
                reader.onload = function(e) {
                    uploadedImages.push({
                        name: file.name,
                        src: e.target.result
                    });
                    
                    updatePreview();
                    generateBtn.disabled = uploadedImages.length === 0;
                };
                
                reader.readAsDataURL(file);
            }
        }
        
        // 更新预览区域
        function updatePreview() {
            previewContainer.innerHTML = '';
            
            uploadedImages.forEach((image, index) => {
                const previewItem = document.createElement('div');
                previewItem.className = 'preview-item';
                
                const img = document.createElement('img');
                img.src = image.src;
                img.alt = `预览 ${index + 1}`;
                
                const removeBtn = document.createElement('div');
                removeBtn.className = 'remove';
                removeBtn.innerHTML = '×';
                removeBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    uploadedImages.splice(index, 1);
                    updatePreview();
                    generateBtn.disabled = uploadedImages.length === 0;
                });
                
                previewItem.appendChild(img);
                previewItem.appendChild(removeBtn);
                previewContainer.appendChild(previewItem);
            });
        }
        
        // 处理生成按钮点击
        function handleGenerate() {
            const count = uploadedImages.length;
            
            if (count === 0) {
                alert('请先上传图片');
                return;
            }
            
            if (count === 6) {
                generateCube();
                return;
            }
            
            // 显示确认对话框
            modalTitle.textContent = `上传了${count}张图片`;
            modalMessage.textContent = getModalMessage(count);
            confirmModal.style.display = 'flex';
        }
        
        // 获取确认对话框消息
        function getModalMessage(count) {
            switch(count) {
                case 1:
                    return '您仅上传了1张图片，是否继续生成？立方体的六个面都将使用这张图片。';
                case 2:
                    return '您仅上传了2张图片，是否继续生成？立方体的六个面将交替使用这两张图片。';
                case 3:
                    return '您仅上传了3张图片，是否继续生成？立方体的六个面将交替使用这三张图片。';
                case 4:
                    return '您仅上传了4张图片，是否继续生成？立方体的前后左右面将使用上传的图片，上下两面将呈现亚克力透明效果。';
                case 5:
                    return '您仅上传了5张图片，是否继续生成？立方体的上下左右前将使用上传的图片，后面将呈现亚克力透明效果。';
                default:
                    return `您上传了${count}张图片，请上传1~6张图片！`;
            }
        }
        
        // 确认生成
        function continueGeneration() {
            confirmModal.style.display = 'none';
            generateCube();
        }
        
        // 取消生成
        function cancelGeneration() {
            confirmModal.style.display = 'none';
            uploadedImages = [];
            updatePreview();
            generateBtn.disabled = true;
        }
        
        // 生成立方体
        function generateCube() {
            const count = uploadedImages.length;
            
            // 重置所有面
            cubeFaces.forEach(face => {
                face.classList.remove('acrylic');
                face.innerHTML = '';
            });
            
            // 根据图片数量设置各个面
            if (count === 1) {
                // 所有面使用同一张图片
                cubeFaces.forEach(face => {
                    const img = document.createElement('img');
                    img.src = uploadedImages[0].src;
                    face.appendChild(img);
                });
            } else if (count === 2) {
                // 交替使用两张图片
                for (let i = 0; i < cubeFaces.length; i++) {
                    const img = document.createElement('img');
                    img.src = uploadedImages[i % 2].src;
                    cubeFaces[i].appendChild(img);
                }
            } else if (count === 3) {
                // 交替使用三张图片
                for (let i = 0; i < cubeFaces.length; i++) {
                    const img = document.createElement('img');
                    img.src = uploadedImages[i % 3].src;
                    cubeFaces[i].appendChild(img);
                }
            } else if (count === 4) {
                // 前后左右使用图片，上下透明
                for (let i = 0; i < 4; i++) {
                    const img = document.createElement('img');
                    img.src = uploadedImages[i].src;
                    cubeFaces[i].appendChild(img);
                }
                cubeFaces[4].classList.add('acrylic');
                cubeFaces[5].classList.add('acrylic');
            } else if (count === 5) {
                // 上下左右前使用图片，后面透明
                for (let i = 0; i < 5; i++) {
                    const img = document.createElement('img');
                    img.src = uploadedImages[i].src;
                    cubeFaces[i].appendChild(img);
                }
                cubeFaces[1].classList.add('acrylic'); // 后面
            } else if (count === 6) {
                // 每个面使用一张图片
                for (let i = 0; i < 6; i++) {
                    const img = document.createElement('img');
                    img.src = uploadedImages[i].src;
                    cubeFaces[i].appendChild(img);
                }
            }
            
            // 显示立方体区域
            switchTab('preview');
        }
        
        // 自动旋转开关
        function toggleAutoRotate() {
            autoRotate = autoRotateCheckbox.checked;
            
            // 清除之前的旋转动画
            if (autoRotateInterval) {
                clearInterval(autoRotateInterval);
                autoRotateInterval = null;
            }
        }
        
        // 立方体旋转控制
        function startDrag(e) {
            isDragging = true;
            startX = e.clientX;
            startY = e.clientY;
        }
        
        function drag(e) {
            if (!isDragging) return;
            
            const deltaX = e.clientX - startX;
            const deltaY = e.clientY - startY;
            
            rotateY += deltaX * 0.5;
            rotateX -= deltaY * 0.5;
            
            updateCubeTransform();
            
            startX = e.clientX;
            startY = e.clientY;
        }
        
        function stopDrag() {
            isDragging = false;
        }
        
        // 更新立方体变换
        function updateCubeTransform() {
            cube.style.transform = `translateZ(-100px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
        }
        
        // 立方体动画
        function animateCube() {
            if (autoRotate && !isDragging) {
                rotateY += 0.5;
                updateCubeTransform();
            }
            animationId = requestAnimationFrame(animateCube);
        }
        
        // 旋转控制按钮功能
        function rotateLeft() {
            if (autoRotate) {
                // 如果自动旋转开启，持续旋转
                if (autoRotateInterval) {
                    clearInterval(autoRotateInterval);
                }
                
                autoRotateInterval = setInterval(() => {
                    rotateY -= 1.5;
                    updateCubeTransform();
                }, 16);
            } else {
                // 如果自动旋转关闭，旋转90度
                rotateY -= 90;
                updateCubeTransform();
            }
        }
        
        function rotateRight() {
            if (autoRotate) {
                // 如果自动旋转开启，持续旋转
                if (autoRotateInterval) {
                    clearInterval(autoRotateInterval);
                }
                
                autoRotateInterval = setInterval(() => {
                    rotateY += 1.5;
                    updateCubeTransform();
                }, 16);
            } else {
                // 如果自动旋转关闭，旋转90度
                rotateY += 90;
                updateCubeTransform();
            }
        }
        
        function resetPosition() {
            rotateX = -20;
            rotateY = -20;
            updateCubeTransform();
            
            // 清除旋转动画
            if (autoRotateInterval) {
                clearInterval(autoRotateInterval);
                autoRotateInterval = null;
            }
        }
        
        // 页面加载时初始化
        window.addEventListener('load', init);
    </script>
</body>
</html>
