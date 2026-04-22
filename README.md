<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>ABS齿圈智能计算器 | 轮胎换装齿数匹配工具</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #0b2b3f 0%, #1a4a6f 100%);
            min-height: 100vh;
            padding: 30px 20px;
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, 'Roboto', sans-serif;
        }

        .calculator-container {
            max-width: 1300px;
            margin: 0 auto;
        }

        /* 头部与语言切换 */
        .header {
            text-align: center;
            margin-bottom: 30px;
            position: relative;
        }
        .lang-switch {
            position: absolute;
            top: 0;
            right: 0;
            background: rgba(255,255,255,0.2);
            border-radius: 40px;
            padding: 5px 12px;
            display: flex;
            gap: 8px;
        }
        .lang-btn {
            background: transparent;
            border: none;
            color: white;
            font-weight: 600;
            cursor: pointer;
            padding: 6px 14px;
            border-radius: 30px;
            transition: 0.2s;
        }
        .lang-btn.active {
            background: #f59e0b;
            color: #1e293b;
        }
        .header h1 {
            font-size: 2.2rem;
            color: white;
            text-shadow: 0 2px 12px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
        }
        .subhead {
            color: rgba(255,255,255,0.9);
            margin-top: 8px;
            font-size: 0.95rem;
        }

        /* 双卡片布局 */
        .cards-row {
            display: flex;
            flex-wrap: wrap;
            gap: 28px;
            margin-bottom: 30px;
        }
        .card {
            flex: 1;
            min-width: 320px;
            background: white;
            border-radius: 32px;
            box-shadow: 0 20px 35px -12px rgba(0,0,0,0.3);
            overflow: hidden;
        }
        .card-header {
            background: #1e293b;
            color: white;
            padding: 18px 24px;
            font-size: 1.3rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .card-header .badge {
            background: #f59e0b;
            padding: 4px 12px;
            border-radius: 40px;
            font-size: 0.7rem;
        }
        .card-body {
            padding: 24px;
        }

        .mode-switch {
            display: flex;
            gap: 12px;
            background: #f1f5f9;
            padding: 6px;
            border-radius: 60px;
            margin-bottom: 28px;
        }
        .mode-btn {
            flex: 1;
            text-align: center;
            padding: 10px 0;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            background: transparent;
            color: #475569;
            border: none;
            font-size: 0.9rem;
        }
        .mode-btn.active {
            background: #3b82f6;
            color: white;
        }
        .input-group {
            margin-bottom: 20px;
        }
        .input-group label {
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 600;
            color: #0f172a;
            margin-bottom: 8px;
            font-size: 0.85rem;
        }
        input {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e2e8f0;
            border-radius: 24px;
            font-size: 0.95rem;
            outline: none;
        }
        input:focus {
            border-color: #3b82f6;
        }
        .inline-group {
            display: flex;
            gap: 12px;
        }
        .inline-group .input-group {
            flex: 1;
        }
        .hint-text {
            font-size: 0.7rem;
            color: #64748b;
            margin-top: 4px;
            margin-left: 8px;
        }

        /* 结果区域 */
        .result-section {
            background: white;
            border-radius: 32px;
            box-shadow: 0 25px 40px -12px rgba(0,0,0,0.35);
            overflow: hidden;
            margin-top: 20px;
        }
        .result-header {
            background: linear-gradient(135deg, #0f172a, #1e293b);
            color: white;
            padding: 20px 28px;
        }
        .result-body {
            padding: 28px;
        }
        .result-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            margin-bottom: 28px;
        }
        .result-card-item {
            background: #f8fafc;
            border-radius: 24px;
            padding: 18px;
            text-align: center;
        }
        .result-label {
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #475569;
            font-weight: 600;
        }
        .result-value {
            font-size: 1.8rem;
            font-weight: 800;
            margin: 8px 0;
        }
        .result-value.waiting {
            color: #94a3b8;
            font-size: 1.2rem;
            font-weight: normal;
        }

        /* 最终结果醒目卡片 */
        .final-result-card {
            background: linear-gradient(145deg, #fef9e6, #fff6e0);
            border-radius: 40px;
            padding: 20px;
            margin: 20px 0;
            border: 2px solid #ffd966;
        }
        .final-badge {
            display: inline-block;
            background: #ff8c00;
            color: white;
            font-weight: bold;
            font-size: 0.75rem;
            padding: 4px 14px;
            border-radius: 60px;
            letter-spacing: 1px;
            margin-bottom: 16px;
        }
        .final-dual-row {
            display: flex;
            flex-wrap: wrap;
            gap: 30px;
            justify-content: center;
        }
        .final-box {
            flex: 1;
            background: white;
            border-radius: 32px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 8px 20px rgba(0,0,0,0.08);
        }
        .final-label {
            font-size: 0.85rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #b45309;
            background: #fff0dd;
            display: inline-block;
            padding: 4px 16px;
            border-radius: 30px;
            margin-bottom: 18px;
        }
        .final-number {
            font-size: 2.8rem;
            font-weight: 900;
        }
        .final-number-rounded {
            background: linear-gradient(135deg, #f97316, #dc2626);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            font-size: 3.2rem;
        }
        .final-number.waiting {
            color: #94a3b8;
            font-size: 1.5rem;
            font-weight: normal;
            background: none;
            -webkit-background-clip: unset;
            background-clip: unset;
        }
        .final-caption {
            font-size: 0.75rem;
            color: #5b5b5b;
            margin-top: 10px;
        }

        /* 按钮组 - 双按钮布局 */
        .button-group {
            display: flex;
            gap: 16px;
            margin: 16px 0;
            flex-wrap: wrap;
        }
        .btn-calc {
            flex: 2;
            background: linear-gradient(135deg, #f97316, #dc2626);
            border: none;
            padding: 18px;
            border-radius: 60px;
            font-size: 1.2rem;
            font-weight: 700;
            color: white;
            cursor: pointer;
            transition: transform 0.2s;
            box-shadow: 0 6px 14px rgba(220,38,38,0.3);
        }
        .btn-share {
            flex: 1;
            background: linear-gradient(135deg, #10b981, #059669);
            border: none;
            padding: 18px;
            border-radius: 60px;
            font-size: 1.1rem;
            font-weight: 700;
            color: white;
            cursor: pointer;
            transition: transform 0.2s;
            box-shadow: 0 6px 14px rgba(16,185,129,0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .btn-calc:hover, .btn-share:hover {
            transform: scale(0.98);
        }
        .toast-message {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: #1e293b;
            color: white;
            padding: 12px 24px;
            border-radius: 50px;
            font-size: 0.9rem;
            z-index: 1000;
            animation: fadeInOut 2s ease-in-out;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            white-space: nowrap;
        }
        @keyframes fadeInOut {
            0% { opacity: 0; transform: translateX(-50%) translateY(20px); }
            15% { opacity: 1; transform: translateX(-50%) translateY(0); }
            85% { opacity: 1; transform: translateX(-50%) translateY(0); }
            100% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
        }
        .info-tip {
            background: #e6f7ff;
            border-left: 5px solid #1890ff;
            padding: 12px 18px;
            border-radius: 20px;
            font-size: 0.85rem;
            margin-top: 16px;
            color: #0050b3;
        }
        .format-example {
            background: #f0f9ff;
            padding: 10px 14px;
            border-radius: 16px;
            margin-top: 12px;
            font-size: 0.75rem;
            color: #0369a1;
            border: 1px solid #bae6fd;
        }
        .format-example span {
            font-weight: 600;
            color: #0c4a6e;
        }
        @media (max-width: 700px) {
            .final-number { font-size: 2rem; }
            .final-number-rounded { font-size: 2.4rem; }
            .btn-calc, .btn-share { padding: 14px; font-size: 1rem; }
            .toast-message { white-space: normal; text-align: center; width: 80%; }
        }
    </style>
</head>
<body>
<div class="calculator-container">
    <div class="header">
        <div class="lang-switch">
            <button class="lang-btn active" data-lang="zh">中文</button>
            <button class="lang-btn" data-lang="en">English</button>
        </div>
        <h1 id="mainTitle">🛞 ABS齿圈智能计算器</h1>
        <div class="subhead" id="subTitle">更换轮胎后自动匹配ABS齿圈齿数 | 自由输入任意轮胎规格</div>
    </div>

    <div class="cards-row">
        <!-- 更换前卡片 -->
        <div class="card">
            <div class="card-header"><span id="beforeTitle">🔄 更换前 · 原配置</span><span class="badge" id="beforeBadge">基准</span></div>
            <div class="card-body">
                <div class="mode-switch" id="modeSwitchBefore">
                    <button class="mode-btn active" data-mode="spec" id="specBtnBefore">📏 轮胎规格</button>
                    <button class="mode-btn" data-mode="mm" id="mmBtnBefore">📐 尺寸(mm/英寸)</button>
                </div>
                <div id="beforeSpecArea">
                    <div class="input-group">
                        <label id="beforeSpecLabel">轮胎规格（公制/英寸）</label>
                        <input type="text" id="beforeTireSpec" placeholder="例: 3.5-10 或 90/90-12" value="3.5-10">
                        <div class="hint-text" id="beforeSpecHint">支持格式: 3.5-10 (英寸制) 或 90/90-12 (公制)</div>
                    </div>
                    <div class="format-example" id="beforeFormatExample">
                        📖 <span>格式说明：</span> 英寸制: "3.5-10" (轮胎宽度-轮毂直径) &nbsp;|&nbsp; 公制: "90/90-12" (宽度/扁平比-轮毂直径)
                    </div>
                </div>
                <div id="beforeMmArea" style="display: none;">
                    <div class="inline-group">
                        <div class="input-group"><label id="beforeWidthLabel">宽度 (mm)</label><input type="number" id="beforeWidthMM" value="92" placeholder="mm"></div>
                        <div class="input-group"><label id="beforeAspectLabel">扁平比 (%)</label><input type="number" id="beforeAspect" value="100" placeholder="%"></div>
                    </div>
                    <div class="input-group"><label id="beforeRimLabel">轮毂直径 (英寸)</label><input type="number" id="beforeRimInch" value="10" step="0.5" placeholder="英寸"></div>
                    <div class="hint-text" id="beforeMmHint">直接输入轮胎宽度、扁平比和轮毂直径</div>
                </div>
                <div class="input-group">
                    <label id="originalTeethLabel">⚙️ 原ABS齿圈齿数</label>
                    <input type="number" id="originalTeeth" value="48" step="1" min="8" max="200">
                    <div class="hint-text" id="originalTeethHint">原厂装配的齿圈齿数，作为计算基准</div>
                </div>
            </div>
        </div>
        <!-- 更换后卡片 -->
        <div class="card">
            <div class="card-header"><span id="afterTitle">🚀 更换后 · 新轮胎</span><span class="badge" id="afterBadge">待匹配</span></div>
            <div class="card-body">
                <div class="mode-switch" id="modeSwitchAfter">
                    <button class="mode-btn active" data-mode="spec" id="specBtnAfter">📏 轮胎规格</button>
                    <button class="mode-btn" data-mode="mm" id="mmBtnAfter">📐 尺寸(mm/英寸)</button>
                </div>
                <div id="afterSpecArea">
                    <div class="input-group">
                        <label id="afterSpecLabel">轮胎规格（公制/英寸）</label>
                        <input type="text" id="afterTireSpec" placeholder="例: 100/80-12 或 120/70-13" value="100/80-12">
                        <div class="hint-text" id="afterSpecHint">支持格式: 3.5-10 (英寸制) 或 90/90-12 (公制)</div>
                    </div>
                    <div class="format-example" id="afterFormatExample">
                        📖 <span>格式说明：</span> 英寸制: "3.5-10" (轮胎宽度-轮毂直径) &nbsp;|&nbsp; 公制: "90/90-12" (宽度/扁平比-轮毂直径)
                    </div>
                </div>
                <div id="afterMmArea" style="display: none;">
                    <div class="inline-group">
                        <div class="input-group"><label id="afterWidthLabel">宽度 (mm)</label><input type="number" id="afterWidthMM" value="100" placeholder="mm"></div>
                        <div class="input-group"><label id="afterAspectLabel">扁平比 (%)</label><input type="number" id="afterAspect" value="80" placeholder="%"></div>
                    </div>
                    <div class="input-group"><label id="afterRimLabel">轮毂直径 (英寸)</label><input type="number" id="afterRimInch" value="12" step="0.5" placeholder="英寸"></div>
                    <div class="hint-text" id="afterMmHint">直接输入轮胎宽度、扁平比和轮毂直径</div>
                </div>
            </div>
        </div>
    </div>

    <!-- 双按钮组：计算 + 分享 -->
    <div class="button-group">
        <button class="btn-calc" id="calculateBtn">⚡ 计算新齿圈数 ⚡</button>
        <button class="btn-share" id="shareBtn">📤 分享给好友</button>
    </div>

    <!-- 结果区 -->
    <div class="result-section">
        <div class="result-header"><h2 id="resultTitle">📊 齿圈计算结论</h2></div>
        <div class="result-body">
            <div class="result-grid">
                <div class="result-card-item"><div class="result-label" id="oldCircLabel">原轮胎滚动周长</div><div class="result-value waiting" id="oldCircum">-- 等待计算 --</div></div>
                <div class="result-card-item"><div class="result-label" id="newCircLabel">新轮胎滚动周长</div><div class="result-value waiting" id="newCircum">-- 等待计算 --</div></div>
                <div class="result-card-item"><div class="result-label" id="deltaLabel">周长变化率</div><div class="result-value waiting" id="deltaPercent">-- 等待计算 --</div></div>
            </div>
            <div class="final-result-card">
                <div style="text-align:center"><span class="final-badge" id="finalBadge">🏆 最终齿圈设计参考</span></div>
                <div class="final-dual-row">
                    <div class="final-box"><div class="final-label" id="exactLabel">🔢 精确计算值</div><div class="final-number waiting" id="exactTeethDisplayBig">-- 等待计算 --</div><div class="final-caption" id="exactCaption">公式：原齿数 × (新周长/原周长)</div></div>
                    <div class="final-box"><div class="final-label" id="roundedLabel">✅ 推荐新齿圈</div><div class="final-number final-number-rounded" id="roundedTeethDisplayBig">-- 等待计算 --</div><div class="final-caption" id="roundedCaption">★ 工程定制建议 ★</div></div>
                </div>
            </div>
            <div class="info-tip" id="infoMessage">💡 请填写轮胎参数后，点击「计算新齿圈数」按钮查看结果。</div>
        </div>
    </div>
</div>

<script>
    // ---------- 完整多语言词库 ----------
    // 分享链接统一使用小写 www.aohetuo.com
    const SHARE_URL = 'www.aohetuo.com';
    
    const langData = {
        zh: {
            mainTitle: "🛞 ABS齿圈智能计算器", 
            subTitle: "更换轮胎后自动匹配ABS齿圈齿数 | 自由输入任意轮胎规格",
            beforeTitle: "🔄 更换前 · 原配置", beforeBadge: "基准",
            afterTitle: "🚀 更换后 · 新轮胎", afterBadge: "待匹配",
            originalTeethLabel: "⚙️ 原ABS齿圈齿数",
            originalTeethHint: "原厂装配的齿圈齿数，作为计算基准",
            beforeSpecLabel: "轮胎规格（公制/英寸）", afterSpecLabel: "轮胎规格（公制/英寸）",
            beforeSpecHint: "支持格式: 3.5-10 (英寸制) 或 90/90-12 (公制)",
            afterSpecHint: "支持格式: 3.5-10 (英寸制) 或 90/90-12 (公制)",
            beforeMmHint: "直接输入轮胎宽度、扁平比和轮毂直径",
            afterMmHint: "直接输入轮胎宽度、扁平比和轮毂直径",
            formatExample: "📖 格式说明： 英寸制: \"3.5-10\" (轮胎宽度-轮毂直径)  |  公制: \"90/90-12\" (宽度/扁平比-轮毂直径)",
            beforeWidthLabel: "宽度 (mm)", beforeAspectLabel: "扁平比 (%)", beforeRimLabel: "轮毂直径 (英寸)",
            afterWidthLabel: "宽度 (mm)", afterAspectLabel: "扁平比 (%)", afterRimLabel: "轮毂直径 (英寸)",
            specBtnBefore: "📏 轮胎规格", mmBtnBefore: "📐 尺寸(mm/英寸)",
            specBtnAfter: "📏 轮胎规格", mmBtnAfter: "📐 尺寸(mm/英寸)",
            resultTitle: "📊 齿圈计算结论",
            oldCircLabel: "原轮胎滚动周长", newCircLabel: "新轮胎滚动周长", deltaLabel: "周长变化率",
            finalBadge: "🏆 最终齿圈设计参考",
            exactLabel: "🔢 精确计算值", roundedLabel: "✅ 推荐新齿圈",
            exactCaption: "公式：原齿数 × (新周长/原周长)", roundedCaption: "★ 工程定制建议 ★",
            waiting: "等待计算",
            saveHint: "请填写轮胎参数后，点击「计算新齿圈数」按钮查看结果。",
            calcBtn: "⚡ 计算新齿圈数 ⚡",
            shareBtn: "📤 分享给好友",
            invalidOld: "请正确填写原轮胎尺寸",
            invalidNew: "请正确填写新轮胎尺寸",
            copySuccess: "✅ 网站地址已复制：www.aohetuo.com",
            copyFailed: "❌ 复制失败，请手动复制"
        },
        en: {
            mainTitle: "🛞 ABS Ring Smart Calculator", 
            subTitle: "Automatic tooth count matching after tire change | Free input for any tire size",
            beforeTitle: "🔄 Before · Original Setup", beforeBadge: "Reference",
            afterTitle: "🚀 After · New Tire", afterBadge: "To Match",
            originalTeethLabel: "⚙️ Original ABS Ring Teeth",
            originalTeethHint: "Original factory ring teeth count, used as calculation base",
            beforeSpecLabel: "Tire Spec (Metric/Inch)", afterSpecLabel: "Tire Spec (Metric/Inch)",
            beforeSpecHint: "Formats: 3.5-10 (inch) or 90/90-12 (metric)",
            afterSpecHint: "Formats: 3.5-10 (inch) or 90/90-12 (metric)",
            beforeMmHint: "Enter tire width, aspect ratio and rim diameter directly",
            afterMmHint: "Enter tire width, aspect ratio and rim diameter directly",
            formatExample: "📖 Format Guide: Inch: \"3.5-10\" (Tire Width-Rim Dia)  |  Metric: \"90/90-12\" (Width/Aspect-Rim Dia)",
            beforeWidthLabel: "Width (mm)", beforeAspectLabel: "Aspect Ratio (%)", beforeRimLabel: "Rim Diameter (inch)",
            afterWidthLabel: "Width (mm)", afterAspectLabel: "Aspect Ratio (%)", afterRimLabel: "Rim Diameter (inch)",
            specBtnBefore: "📏 Tire Spec", mmBtnBefore: "📐 Size (mm/inch)",
            specBtnAfter: "📏 Tire Spec", mmBtnAfter: "📐 Size (mm/inch)",
            resultTitle: "📊 Calculation Result",
            oldCircLabel: "Old Tire Rolling Circ.", newCircLabel: "New Tire Rolling Circ.", deltaLabel: "Circumference Change",
            finalBadge: "🏆 Final Ring Design Reference",
            exactLabel: "🔢 Exact Value", roundedLabel: "✅ Recommended New Ring",
            exactCaption: "Formula: Original Teeth × (New Circ / Old Circ)", roundedCaption: "★ Recommended for Manufacturing ★",
            waiting: "Waiting for calculation",
            saveHint: "Fill in tire parameters, then click 'Calculate New Ring Teeth' to see results.",
            calcBtn: "⚡ Calculate New Ring Teeth ⚡",
            shareBtn: "📤 Share",
            invalidOld: "Please fill in valid original tire size",
            invalidNew: "Please fill in valid new tire size",
            copySuccess: "✅ Website copied: www.aohetuo.com",
            copyFailed: "❌ Copy failed, please copy manually"
        }
    };
    
    let currentLang = 'zh';
    
    function updateUILang() {
        const t = langData[currentLang];
        document.getElementById('mainTitle').innerHTML = t.mainTitle;
        document.getElementById('subTitle').innerHTML = t.subTitle;
        document.getElementById('beforeTitle').innerHTML = t.beforeTitle;
        document.getElementById('beforeBadge').innerHTML = t.beforeBadge;
        document.getElementById('afterTitle').innerHTML = t.afterTitle;
        document.getElementById('afterBadge').innerHTML = t.afterBadge;
        document.getElementById('originalTeethLabel').innerHTML = t.originalTeethLabel;
        document.getElementById('originalTeethHint').innerHTML = t.originalTeethHint;
        document.getElementById('beforeSpecLabel').innerHTML = t.beforeSpecLabel;
        document.getElementById('afterSpecLabel').innerHTML = t.afterSpecLabel;
        document.getElementById('beforeSpecHint').innerHTML = t.beforeSpecHint;
        document.getElementById('afterSpecHint').innerHTML = t.afterSpecHint;
        document.getElementById('beforeMmHint').innerHTML = t.beforeMmHint;
        document.getElementById('afterMmHint').innerHTML = t.afterMmHint;
        document.getElementById('beforeFormatExample').innerHTML = t.formatExample;
        document.getElementById('afterFormatExample').innerHTML = t.formatExample;
        document.getElementById('beforeWidthLabel').innerHTML = t.beforeWidthLabel;
        document.getElementById('beforeAspectLabel').innerHTML = t.beforeAspectLabel;
        document.getElementById('beforeRimLabel').innerHTML = t.beforeRimLabel;
        document.getElementById('afterWidthLabel').innerHTML = t.afterWidthLabel;
        document.getElementById('afterAspectLabel').innerHTML = t.afterAspectLabel;
        document.getElementById('afterRimLabel').innerHTML = t.afterRimLabel;
        document.getElementById('specBtnBefore').innerHTML = t.specBtnBefore;
        document.getElementById('mmBtnBefore').innerHTML = t.mmBtnBefore;
        document.getElementById('specBtnAfter').innerHTML = t.specBtnAfter;
        document.getElementById('mmBtnAfter').innerHTML = t.mmBtnAfter;
        document.getElementById('resultTitle').innerHTML = t.resultTitle;
        document.getElementById('oldCircLabel').innerHTML = t.oldCircLabel;
        document.getElementById('newCircLabel').innerHTML = t.newCircLabel;
        document.getElementById('deltaLabel').innerHTML = t.deltaLabel;
        document.getElementById('finalBadge').innerHTML = t.finalBadge;
        document.getElementById('exactLabel').innerHTML = t.exactLabel;
        document.getElementById('roundedLabel').innerHTML = t.roundedLabel;
        document.getElementById('exactCaption').innerHTML = t.exactCaption;
        document.getElementById('roundedCaption').innerHTML = t.roundedCaption;
        document.getElementById('calculateBtn').innerHTML = t.calcBtn;
        document.getElementById('shareBtn').innerHTML = t.shareBtn;
        document.getElementById('infoMessage').innerHTML = `💡 ${t.saveHint}`;
        
        // 如果当前显示的是等待状态，更新等待文字
        const oldCircum = document.getElementById('oldCircum');
        if (oldCircum.classList.contains('waiting')) {
            oldCircum.innerHTML = `-- ${t.waiting} --`;
            document.getElementById('newCircum').innerHTML = `-- ${t.waiting} --`;
            document.getElementById('deltaPercent').innerHTML = `-- ${t.waiting} --`;
            document.getElementById('exactTeethDisplayBig').innerHTML = `-- ${t.waiting} --`;
            document.getElementById('roundedTeethDisplayBig').innerHTML = `-- ${t.waiting} --`;
        }
    }
    
    document.querySelectorAll('.lang-btn').forEach(btn => {
        btn.addEventListener('click', () => {
            currentLang = btn.getAttribute('data-lang');
            document.querySelectorAll('.lang-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            updateUILang();
        });
    });
    updateUILang();

    // 模式切换 (前后独立)
    let beforeUseSpec = true, afterUseSpec = true;
    const beforeSpecArea = document.getElementById('beforeSpecArea'), beforeMmArea = document.getElementById('beforeMmArea');
    const afterSpecArea = document.getElementById('afterSpecArea'), afterMmArea = document.getElementById('afterMmArea');
    function setBeforeMode(useSpec) {
        beforeUseSpec = useSpec;
        beforeSpecArea.style.display = useSpec ? 'block' : 'none';
        beforeMmArea.style.display = useSpec ? 'none' : 'block';
        document.querySelectorAll('#modeSwitchBefore .mode-btn').forEach(btn => {
            const mode = btn.getAttribute('data-mode');
            if((mode==='spec' && useSpec) || (mode==='mm' && !useSpec)) btn.classList.add('active');
            else btn.classList.remove('active');
        });
    }
    function setAfterMode(useSpec) {
        afterUseSpec = useSpec;
        afterSpecArea.style.display = useSpec ? 'block' : 'none';
        afterMmArea.style.display = useSpec ? 'none' : 'block';
        document.querySelectorAll('#modeSwitchAfter .mode-btn').forEach(btn => {
            const mode = btn.getAttribute('data-mode');
            if((mode==='spec' && useSpec) || (mode==='mm' && !useSpec)) btn.classList.add('active');
            else btn.classList.remove('active');
        });
    }
    document.querySelectorAll('#modeSwitchBefore .mode-btn').forEach(btn => btn.addEventListener('click',()=>{ setBeforeMode(btn.getAttribute('data-mode')==='spec'); }));
    document.querySelectorAll('#modeSwitchAfter .mode-btn').forEach(btn => btn.addEventListener('click',()=>{ setAfterMode(btn.getAttribute('data-mode')==='spec'); }));
    setBeforeMode(true); setAfterMode(true);

    // 显示提示消息（自动消失）
    function showToast(message) {
        // 移除已有的toast
        const existingToast = document.querySelector('.toast-message');
        if(existingToast) existingToast.remove();
        
        const toast = document.createElement('div');
        toast.className = 'toast-message';
        toast.textContent = message;
        document.body.appendChild(toast);
        
        setTimeout(() => {
            if(toast) toast.remove();
        }, 2000);
    }

    // 分享功能：复制小写网站地址 www.aohetuo.com
    function shareWebsite() {
        const t = langData[currentLang];
        
        // 使用现代 Clipboard API
        if (navigator.clipboard && navigator.clipboard.writeText) {
            navigator.clipboard.writeText(SHARE_URL).then(() => {
                showToast(t.copySuccess);
            }).catch(() => {
                // 降级方案
                fallbackCopy(SHARE_URL, t);
            });
        } else {
            fallbackCopy(SHARE_URL, t);
        }
    }
    
    // 降级复制方案
    function fallbackCopy(text, t) {
        const textarea = document.createElement('textarea');
        textarea.value = text;
        textarea.style.position = 'fixed';
        textarea.style.opacity = '0';
        document.body.appendChild(textarea);
        textarea.select();
        try {
            document.execCommand('copy');
            showToast(t.copySuccess);
        } catch (err) {
            showToast(t.copyFailed);
        }
        document.body.removeChild(textarea);
    }

    // 轮胎解析
    function parseTireSpec(specStr) {
        if(!specStr) return { valid: false };
        let s = specStr.trim().replace(/\s/g,'');
        // 公制格式: 宽度/扁平比-轮毂直径
        let metric = s.match(/^(\d+)\/(\d+)-(\d+(?:\.\d+)?)$/);
        if(metric){
            let w=parseFloat(metric[1]), a=parseFloat(metric[2]), r=parseFloat(metric[3]);
            if(w>0 && a>0 && r>0) return { valid:true, widthMM:w, aspectRatio:a, rimInch:r };
        }
        // 英寸制格式: 轮胎宽度-轮毂直径
        let inch = s.match(/^(\d+(?:\.\d+)?)-(\d+(?:\.\d+)?)$/);
        if(inch){
            let wInch=parseFloat(inch[1]), r=parseFloat(inch[2]);
            if(wInch>0 && r>0) return { valid:true, widthMM:wInch*25.4, aspectRatio:85, rimInch:r };
        }
        return { valid:false };
    }
    function calcCirc(widthMM, aspect, rimInch){
        if(widthMM<=0) return null;
        let side = widthMM * (aspect/100);
        let rimMM = rimInch * 25.4;
        let outer = rimMM + 2*side;
        if(outer<=0) return null;
        return Math.PI * outer;
    }
    function getBeforeCirc(){
        if(beforeUseSpec){
            let parsed = parseTireSpec(document.getElementById('beforeTireSpec').value);
            if(parsed.valid) return calcCirc(parsed.widthMM, parsed.aspectRatio, parsed.rimInch);
        }
        let w = parseFloat(document.getElementById('beforeWidthMM').value);
        let a = parseFloat(document.getElementById('beforeAspect').value);
        let r = parseFloat(document.getElementById('beforeRimInch').value);
        if(!isNaN(w) && w>0 && !isNaN(a) && a>0 && !isNaN(r) && r>0) return calcCirc(w,a,r);
        return null;
    }
    function getAfterCirc(){
        if(afterUseSpec){
            let parsed = parseTireSpec(document.getElementById('afterTireSpec').value);
            if(parsed.valid) return calcCirc(parsed.widthMM, parsed.aspectRatio, parsed.rimInch);
        }
        let w = parseFloat(document.getElementById('afterWidthMM').value);
        let a = parseFloat(document.getElementById('afterAspect').value);
        let r = parseFloat(document.getElementById('afterRimInch').value);
        if(!isNaN(w) && w>0 && !isNaN(a) && a>0 && !isNaN(r) && r>0) return calcCirc(w,a,r);
        return null;
    }

    // 核心计算函数 - 只有点击按钮时才执行
    function compute(){
        let oldCirc = getBeforeCirc();
        let newCirc = getAfterCirc();
        let originalTeeth = parseFloat(document.getElementById('originalTeeth').value);
        if(isNaN(originalTeeth) || originalTeeth<=0) originalTeeth = 48;
        
        const t = langData[currentLang];
        
        // 检查数据有效性
        if(!oldCirc || oldCirc <= 0){
            document.getElementById('oldCircum').innerHTML = '❌ 无效';
            document.getElementById('newCircum').innerHTML = '-- mm';
            document.getElementById('deltaPercent').innerHTML = '-- %';
            document.getElementById('exactTeethDisplayBig').innerHTML = '❌ 无效';
            document.getElementById('roundedTeethDisplayBig').innerHTML = '❌ 无效';
            document.getElementById('infoMessage').innerHTML = `⚠️ ${t.invalidOld}！${t.saveHint}`;
            return;
        }
        if(!newCirc || newCirc <= 0){
            document.getElementById('oldCircum').innerHTML = oldCirc.toFixed(1)+' mm';
            document.getElementById('newCircum').innerHTML = '❌ 无效';
            document.getElementById('deltaPercent').innerHTML = '-- %';
            document.getElementById('exactTeethDisplayBig').innerHTML = '❌ 无效';
            document.getElementById('roundedTeethDisplayBig').innerHTML = '❌ 无效';
            document.getElementById('infoMessage').innerHTML = `⚠️ ${t.invalidNew}！${t.saveHint}`;
            return;
        }
        
        // 计算结果
        let ratio = newCirc / oldCirc;
        let exact = originalTeeth * ratio;
        let rounded = Math.round(exact);
        if(rounded<8) rounded=8;
        if(rounded>250) rounded=250;
        let percent = (ratio-1)*100;
        
        // 更新UI显示结果（移除waiting样式）
        const oldCircumEl = document.getElementById('oldCircum');
        const newCircumEl = document.getElementById('newCircum');
        const deltaPercentEl = document.getElementById('deltaPercent');
        const exactTeethEl = document.getElementById('exactTeethDisplayBig');
        const roundedTeethEl = document.getElementById('roundedTeethDisplayBig');
        
        oldCircumEl.classList.remove('waiting');
        newCircumEl.classList.remove('waiting');
        deltaPercentEl.classList.remove('waiting');
        exactTeethEl.classList.remove('waiting');
        roundedTeethEl.classList.remove('waiting');
        
        oldCircumEl.innerHTML = oldCirc.toFixed(1)+' mm';
        newCircumEl.innerHTML = newCirc.toFixed(1)+' mm';
        deltaPercentEl.innerHTML = percent.toFixed(2)+' %';
        exactTeethEl.innerHTML = exact.toFixed(3)+' 齿';
        roundedTeethEl.innerHTML = rounded+' 齿';
        
        // 生成公式提示
        let formulaMsg = currentLang==='zh'? 
            `📐 计算依据：${originalTeeth} × (${newCirc.toFixed(1)} / ${oldCirc.toFixed(1)}) = ${exact.toFixed(3)} 齿 → 推荐新齿圈 ${rounded} 齿` :
            `📐 Formula: ${originalTeeth} × (${newCirc.toFixed(1)} / ${oldCirc.toFixed(1)}) = ${exact.toFixed(3)} teeth → Recommended new ring: ${rounded} teeth`;
        document.getElementById('infoMessage').innerHTML = `💡 ${formulaMsg}`;
    }

    // 绑定计算按钮事件
    document.getElementById('calculateBtn').addEventListener('click', compute);
    // 绑定分享按钮事件
    document.getElementById('shareBtn').addEventListener('click', shareWebsite);
    
    // 页面加载时只设置等待状态，不进行计算
    window.addEventListener('DOMContentLoaded', () => { 
        const t = langData[currentLang];
        document.getElementById('oldCircum').innerHTML = `-- ${t.waiting} --`;
        document.getElementById('newCircum').innerHTML = `-- ${t.waiting} --`;
        document.getElementById('deltaPercent').innerHTML = `-- ${t.waiting} --`;
        document.getElementById('exactTeethDisplayBig').innerHTML = `-- ${t.waiting} --`;
        document.getElementById('roundedTeethDisplayBig').innerHTML = `-- ${t.waiting} --`;
    });
</script>
</body>
</html>
