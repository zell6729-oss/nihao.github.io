<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>形式的综合与语境的拟合：从“看见”到“洞察”</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        stone: {
                            50: '#fafaf9',
                            100: '#f5f5f4',
                            200: '#e7e5e4',
                            800: '#292524',
                            900: '#1c1917',
                        },
                        terracotta: '#c2410c',
                        sage: '#4d7c0f',
                        clay: '#b45309',
                        sand: '#d6d3d1'
                    },
                    fontFamily: {
                        sans: ['"Helvetica Neue"', 'Helvetica', 'Arial', '"Microsoft YaHei"', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #fafaf9;
            color: #292524;
            overflow-x: hidden;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 700px;
            margin-left: auto;
            margin-right: auto;
            height: 380px;
            max-height: 420px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 450px;
            }
        }
        
        /* 3D Flip Card Styles */
        .flip-card {
            background-color: transparent;
            width: 100%;
            height: 450px;
            perspective: 1000px;
            cursor: pointer;
        }
        .flip-card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: left;
            transition: transform 0.8s;
            transform-style: preserve-3d;
        }
        .flip-card.flipped .flip-card-inner {
            transform: rotateY(180deg);
        }
        .flip-card-front, .flip-card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
            border-radius: 1rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            padding: 2rem;
            display: flex;
            flex-direction: column;
            border: 1px solid #e7e5e4;
        }
        .flip-card-front {
            background-color: #ffffff;
            color: #292524;
            z-index: 2;
        }
        .flip-card-back {
            background-color: #292524;
            color: #fafaf9;
            transform: rotateY(180deg);
            overflow-y: auto;
        }
        .flip-card-back::-webkit-scrollbar {
            width: 6px;
        }
        .flip-card-back::-webkit-scrollbar-track {
            background: #1c1917; 
            border-radius: 4px;
        }
        .flip-card-back::-webkit-scrollbar-thumb {
            background: #c2410c; 
            border-radius: 4px;
        }

        .insight-highlight {
            border-left: 4px solid #c2410c;
            padding-left: 1rem;
            margin-top: 1rem;
            background-color: rgba(194, 65, 12, 0.1);
            padding: 1rem;
            border-radius: 0 0.5rem 0.5rem 0;
        }

        .interactive-tab.active {
            background-color: #c2410c;
            color: white;
            border-color: #c2410c;
        }
    </style>
</head>
<body class="antialiased font-sans leading-relaxed">

    <!-- Chosen Palette: Warm Neutrals & Earth Tones (Stone background, Terracotta, Sage, Clay accents) -->
    <!-- Application Structure Plan: 
         1. Hero Section: Introduces the core philosophy of Christopher Alexander (Form vs Context, Good Fit).
         2. Interactive Flip Cards (The Core): Addresses the user's specific request to correct the cases based on the PDF. Uses 3D flip cards to physically manifest the transition from "Seeing the Form" (Front) to "Perceiving the Truth/Rationale" (Back). Emphasizes the requested "因为...所以..." structure.
         3. Dynamic Spectrum Analysis: An interactive section where users click tabs to see how the "Context" shifts from survival to emotion, updating a text description and highlighting a specific aspect of the Radar Chart.
         4. Multi-dimensional Radar Chart: Quantifies the abstract design theories (Utility vs Emotion vs Environment) across the 3 cases.
    -->
    <!-- Visualization & Content Choices:
         - 3D Flip Cards (HTML/CSS/JS): Goal: Inform & Engage. Method: CSS 3D transforms. Justification: The physical flip metaphor perfectly matches the cognitive leap from superficial "Form" to deep "Truth" requested in the PDF.
         - Interactive Spectrum Tabs (HTML/JS): Goal: Organize & Explain. Method: Clickable buttons updating text and chart highlights. Justification: Breaks down the "Evolution Spectrum" from the PDF conclusion into digestible, interactive chunks.
         - Radar Chart (Chart.js): Goal: Compare. Method: Radar chart showing 5 dimensions (Utility, Emotion, Environment, Rebellion, Fit). Justification: Effectively compares objects across drastically different contexts (Nature vs Kitchen vs Social dining). Library: Chart.js Canvas. NO SVG used.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <header class="bg-stone-900 text-stone-50 py-20 px-4 sm:px-6 lg:px-8 border-b-[6px] border-terracotta relative overflow-hidden">
        <div class="absolute inset-0 opacity-10" style="background-image: radial-gradient(#d6d3d1 1px, transparent 1px); background-size: 20px 20px;"></div>
        <div class="max-w-5xl mx-auto text-center relative z-10">
            <span class="text-terracotta font-bold tracking-widest uppercase text-sm mb-4 block">认知论飞跃 / Epistemological Leap</span>
            <h1 class="text-4xl md:text-6xl font-extrabold mb-6 tracking-tight leading-tight">形式的综合与语境的拟合</h1>
            <p class="text-xl md:text-2xl text-stone-300 max-w-3xl mx-auto font-light leading-relaxed mb-8">
                “设计的终极奥义在于建立存在的理由。”
            </p>
            <p class="text-base text-stone-400 max-w-2xl mx-auto">
                基于克里斯托弗·亚历山大的理论：设计是努力实现“形式（相）”与“语境（象）”之间的完美契合。点击下方卡片，跨越视觉的表象，洞察设计的真相。
            </p>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16 space-y-24">

        <section id="interactive-cases">
            <div class="text-center mb-12">
                <h2 class="text-3xl font-bold text-stone-800 mb-4 flex items-center justify-center gap-3">
                    <span class="text-2xl">🔍</span> 深度解构：点击翻转卡片洞察真相
                </h2>
                <p class="text-stone-600 max-w-2xl mx-auto">
                    每一件造物都在解决“未有与已有”、“已知与未知”的转化。观察正面的形态表征，点击翻转以揭示背后以“因为……所以……”为逻辑的设计理由。
                </p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                
                <!-- Case 1: Juicy Salif -->
                <div class="flip-card" onclick="this.classList.toggle('flipped')">
                    <div class="flip-card-inner">
                        <div class="flip-card-front">
                            <div class="text-5xl mb-6 text-center">🛸</div>
                            <h3 class="text-2xl font-bold text-stone-800 mb-2 border-b-2 border-stone-100 pb-2">Juicy Salif 榨汁机</h3>
                            <h4 class="text-xs font-bold text-terracotta uppercase tracking-wider mt-4 mb-2">形态表征 (Form)</h4>
                            <ul class="text-stone-600 text-sm space-y-2 flex-grow list-disc pl-4">
                                <li><strong>无收集底座：</strong> 剥离了传统榨汁机容纳果汁的容器。</li>
                                <li><strong>高耸三足鼎立：</strong> 宛如外星生物触手，极具入侵感与压迫感。</li>
                                <li><strong>镜面抛光铝材：</strong> 冷峻、锐利，视觉上制造危险与科技感。</li>
                            </ul>
                            <div class="mt-4 text-center text-stone-400 text-xs flex items-center justify-center gap-2">
                                <span>👆 点击翻转洞察真相</span>
                            </div>
                        </div>
                        <div class="flip-card-back border-t-4 border-terracotta">
                            <h3 class="text-xl font-bold text-white mb-4 border-b border-stone-700 pb-2">功能主义的叛逆</h3>
                            <h4 class="text-xs font-bold text-sage uppercase tracking-wider mb-2">真实语境 (Context)</h4>
                            <p class="text-stone-300 text-sm mb-4">
                                它在榨汁时极易飞溅，缺乏滤网，甚至尖锐伤人。斯塔克面对的并非“如何高效榨汁”的物理痛点，而是中产阶级厨房的情感荒芜（审美疲劳）。
                            </p>
                            <h4 class="text-xs font-bold text-clay uppercase tracking-wider mb-2">设计理由 (Rationale)</h4>
                            <div class="insight-highlight text-sm text-stone-200">
                                <strong>因为</strong> 它的真正功能是提供快乐（Joy is a function）并作为餐桌上的“社交媒介”来开启对话，<br><br>
                                <strong>所以</strong> 斯塔克刻意制造了物理功能上的“失配（Misfit）”，用极其夸张的反叛形态牺牲了实用性，精准引爆了用户的“兴奋点”，使其成为一件诚实的微型雕塑。
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Case 2: Swanky Ladle -->
                <div class="flip-card" onclick="this.classList.toggle('flipped')">
                    <div class="flip-card-inner">
                        <div class="flip-card-front">
                            <div class="text-5xl mb-6 text-center">🦢</div>
                            <h3 class="text-2xl font-bold text-stone-800 mb-2 border-b-2 border-stone-100 pb-2">Swanky 悬浮汤勺</h3>
                            <h4 class="text-xs font-bold text-terracotta uppercase tracking-wider mt-4 mb-2">形态表征 (Form)</h4>
                            <ul class="text-stone-600 text-sm space-y-2 flex-grow list-disc pl-4">
                                <li><strong>仿生S型曲线：</strong> 握把极度修长，模拟天鹅颈部的优雅姿态。</li>
                                <li><strong>膨胀半球形勺体：</strong> 犹如天鹅丰满的躯干，体积经过精确计算。</li>
                                <li><strong>底部隐藏配重：</strong> 内部嵌合不锈钢核心，实现物理上的自平衡。</li>
                            </ul>
                            <div class="mt-4 text-center text-stone-400 text-xs flex items-center justify-center gap-2">
                                <span>👆 点击翻转洞察真相</span>
                            </div>
                        </div>
                        <div class="flip-card-back border-t-4 border-sage">
                            <h3 class="text-xl font-bold text-white mb-4 border-b border-stone-700 pb-2">物理与诗意的弥合</h3>
                            <h4 class="text-xs font-bold text-sage uppercase tracking-wider mb-2">真实语境 (Context)</h4>
                            <p class="text-stone-300 text-sm mb-4">
                                传统汤勺极易滑入深锅底部，造成手柄黏腻的卫生问题（痛点）；且平放时占用台面空间，缺乏美感（痒点）。
                            </p>
                            <h4 class="text-xs font-bold text-clay uppercase tracking-wider mb-2">设计理由 (Rationale)</h4>
                            <div class="insight-highlight text-sm text-stone-200">
                                <strong>因为</strong> 设计师需要彻底消除汤勺沉没的痛点并满足厨房收纳美学，<br><br>
                                <strong>所以</strong> 利用阿基米德浮力原理排开液体，结合底部配重的低重心设计，让汤勺能在汤面或台面上永远保持直立。将枯燥的流体力学包裹在天鹅的仿生形态中，达成了功能与形式的完美契合（Good Fit）。
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Case 3: Alpine Roof -->
                <div class="flip-card" onclick="this.classList.toggle('flipped')">
                    <div class="flip-card-inner">
                        <div class="flip-card-front">
                            <div class="text-5xl mb-6 text-center">🏔️</div>
                            <h3 class="text-2xl font-bold text-stone-800 mb-2 border-b-2 border-stone-100 pb-2">阿尔卑斯石质屋顶</h3>
                            <h4 class="text-xs font-bold text-terracotta uppercase tracking-wider mt-4 mb-2">形态表征 (Form)</h4>
                            <ul class="text-stone-600 text-sm space-y-2 flex-grow list-disc pl-4">
                                <li><strong>超大质量天然石块：</strong> 厚重、质感粗糙的片麻岩或板岩层层堆叠。</li>
                                <li><strong>无钉化重力构造：</strong> 惊人地没有使用任何铁钉或金属紧固件。</li>
                                <li><strong>特定坡度与粗糙度：</strong> 有些坡度出人意料地平缓，且表面极不光滑，能留住大量积雪。</li>
                            </ul>
                            <div class="mt-4 text-center text-stone-400 text-xs flex items-center justify-center gap-2">
                                <span>👆 点击翻转洞察真相</span>
                            </div>
                        </div>
                        <div class="flip-card-back border-t-4 border-stone-400">
                            <h3 class="text-xl font-bold text-white mb-4 border-b border-stone-700 pb-2">硬核实用主义生存</h3>
                            <h4 class="text-xs font-bold text-sage uppercase tracking-wider mb-2">真实语境 (Context)</h4>
                            <p class="text-stone-300 text-sm mb-4">
                                面临高山狂风肆虐、极端严寒暴雪，以及历史时期铁矿资源极度匮乏（铁钉极其昂贵）的生死存亡考验。
                            </p>
                            <h4 class="text-xs font-bold text-clay uppercase tracking-wider mb-2">设计理由 (Rationale)</h4>
                            <div class="insight-highlight text-sm text-stone-200">
                                <strong>因为</strong> 在狂风中保护庇护所是底线，且古代山民买不起铁钉，<br><br>
                                <strong>所以</strong> 采用超重石板依靠重力与摩擦力进行无钉化堆叠来抗风；又 <strong>因为</strong> 必须抵御漫长严寒，<strong>所以</strong> 刻意利用粗糙表面阻滞冰雪滑落，将积雪转化为天然的绝佳保温层，展现了顺应自然法则的最高级“无意识设计”。
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </section>

        <section id="analysis-dashboard" class="bg-white rounded-2xl shadow-sm border border-stone-200 p-8">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-stone-800 mb-4">真相演变谱系与力场量化</h2>
                <p class="text-stone-600 max-w-2xl mx-auto">
                    随着人类文明发展，设计对抗的“语境”发生位移。点击下方阶段，观察设计力场如何从生存刚需演变为精神狂欢。
                </p>
            </div>

            <div class="flex flex-wrap justify-center gap-4 mb-10">
                <button class="interactive-tab active px-6 py-2 rounded-full border border-stone-300 text-stone-600 font-semibold transition-colors hover:bg-stone-100" data-stage="survival">阶段一：生存刚需</button>
                <button class="interactive-tab px-6 py-2 rounded-full border border-stone-300 text-stone-600 font-semibold transition-colors hover:bg-stone-100" data-stage="painpoint">阶段二：痛点消解</button>
                <button class="interactive-tab px-6 py-2 rounded-full border border-stone-300 text-stone-600 font-semibold transition-colors hover:bg-stone-100" data-stage="emotion">阶段三：精神狂欢</button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
                
                <div id="dynamic-text-container" class="bg-stone-50 p-6 rounded-xl border border-stone-200 h-full flex flex-col justify-center transition-opacity duration-300">
                    <!-- Dynamic Content Inserted via JS -->
                </div>

                <div class="w-full">
                    <h3 class="text-center font-bold text-stone-800 mb-2">造物形态多维力场雷达图</h3>
                    <p class="text-center text-xs text-stone-500 mb-6">展示不同形态在实用、情感、抗压等维度的极化分布</p>
                    <div class="chart-container">
                        <canvas id="radarChart"></canvas>
                    </div>
                </div>

            </div>
        </section>

        <section id="conclusion" class="max-w-4xl mx-auto text-center border-t border-stone-200 pt-16">
            <h2 class="text-3xl font-bold text-stone-800 mb-6">总结：建立存在的理由</h2>
            <p class="text-lg text-stone-600 leading-relaxed text-left">
                三个案例无一例外地证明了：<strong>当我们在谈论设计时，真正讨论的并非单一的形态表征（相），而是形式与其语境共同构成的整体（象）。</strong>
            </p>
            <div class="mt-8 grid grid-cols-1 md:grid-cols-3 gap-6 text-left">
                <div class="bg-stone-100 p-6 rounded-lg border-l-4 border-stone-500">
                    <p class="text-sm text-stone-700"><strong>石质屋顶的“相”是粗糙沉重的</strong>，因为它对应的“象”是狂风暴雪的生死考验。</p>
                </div>
                <div class="bg-stone-100 p-6 rounded-lg border-l-4 border-sage">
                    <p class="text-sm text-stone-700"><strong>Swanky汤勺的“相”是灵动漂浮的</strong>，因为它对应的“象”是厨房卫生的痛点与秩序诉求。</p>
                </div>
                <div class="bg-stone-100 p-6 rounded-lg border-l-4 border-terracotta">
                    <p class="text-sm text-stone-700"><strong>Juicy Salif的“相”是尖锐惊骇的</strong>，因为它对应的“象”是需要被打破的沉闷中产生活与极简主义教条。</p>
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-stone-900 text-stone-400 py-12 text-center mt-12">
        <div class="max-w-4xl mx-auto px-4">
            <p class="mb-2 font-semibold">基于《形式的综合附记》与《设计形态与背后真相分析》构建</p>
            <p class="text-sm">格物致知 · 知行合一</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            // --- Chart.js Required Configuration & Wrapping Logic ---
            function wrapLabel(label, maxLength = 16) {
                if (label.length <= maxLength) return label;
                const words = label.split(' ');
                const lines = [];
                let currentLine = '';
                words.forEach(word => {
                    if ((currentLine + word).length > maxLength) {
                        if (currentLine) lines.push(currentLine.trim());
                        currentLine = word + ' ';
                    } else {
                        currentLine += word + ' ';
                    }
                });
                if (currentLine) lines.push(currentLine.trim());
                return lines;
            }

            const standardTooltipConfig = {
                callbacks: {
                    title: function(tooltipItems) {
                        const item = tooltipItems[0];
                        let label = item.chart.data.labels[item.dataIndex];
                        if (Array.isArray(label)) return label.join(' ');
                        return label;
                    }
                },
                backgroundColor: 'rgba(28, 25, 23, 0.9)',
                titleFont: { size: 14, family: "'Helvetica Neue', sans-serif" },
                bodyFont: { size: 13, family: "'Helvetica Neue', sans-serif" },
                padding: 12,
                cornerRadius: 8
            };

            // --- Radar Chart Initialization ---
            const ctxRadar = document.getElementById('radarChart').getContext('2d');
            const labelsRaw = [
                '物理生存/实用抗压 (Utility)', 
                '情感刺激/话题性 (Emotion)', 
                '环境压力服从度 (Env Submit)', 
                '功能形态反叛度 (Rebellion)', 
                '自然仿生融合度 (Biomimicry)'
            ];
            
            let radarChart = new Chart(ctxRadar, {
                type: 'radar',
                data: {
                    labels: labelsRaw.map(l => wrapLabel(l)),
                    datasets: [
                        {
                            label: 'Juicy Salif (情感狂欢)',
                            data: [10, 100, 10, 100, 20],
                            backgroundColor: 'rgba(194, 65, 12, 0.2)', // Terracotta
                            borderColor: '#c2410c',
                            pointBackgroundColor: '#c2410c',
                            borderWidth: 2,
                            hidden: true // Start hidden to allow interactive revealing
                        },
                        {
                            label: 'Swanky 汤勺 (痛点消解)',
                            data: [90, 80, 40, 30, 95],
                            backgroundColor: 'rgba(77, 124, 15, 0.2)', // Sage
                            borderColor: '#4d7c0f',
                            pointBackgroundColor: '#4d7c0f',
                            borderWidth: 2,
                            hidden: true
                        },
                        {
                            label: '阿尔卑斯屋顶 (生存刚需)',
                            data: [100, 10, 100, 0, 100],
                            backgroundColor: 'rgba(168, 162, 158, 0.4)', // Stone
                            borderColor: '#78716c',
                            pointBackgroundColor: '#78716c',
                            borderWidth: 2,
                            hidden: false // Show first by default
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: { color: 'rgba(41, 37, 36, 0.1)' },
                            grid: { color: 'rgba(41, 37, 36, 0.1)' },
                            pointLabels: {
                                color: '#57534e',
                                font: { size: 11, family: "'Helvetica Neue', sans-serif", weight: 'bold' }
                            },
                            ticks: { display: false, min: 0, max: 100 }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                color: '#292524',
                                font: { family: "'Helvetica Neue', sans-serif" },
                                usePointStyle: true,
                                padding: 20
                            }
                        },
                        tooltip: standardTooltipConfig
                    }
                }
            });

            // --- Interactive Spectrum Logic ---
            const stageData = {
                survival: {
                    title: "阶段一：生存的刚需与绝对服从",
                    icon: "🏔️",
                    desc: "代表案例：阿尔卑斯石质屋顶",
                    content: "在无意识设计时代，人类面临最底层的生理与安全需求。形态是对严酷自然法则（狂风、暴雪）的直接回应。这里的形态就是功能本身，没有任何讨价还价的余地。因为资源匮乏，所以只能采用绝对的物理重力堆叠来抗衡自然。",
                    datasetIndex: 2
                },
                painpoint: {
                    title: "阶段二：微观痛点消解与诗意平衡",
                    icon: "🦢",
                    desc: "代表案例：Swanky 天鹅悬浮汤勺",
                    content: "随着技术（材料学、流体力学）进步，设计向下渗透至日常生活的微观痛点（如汤勺沉没）。形态不再仅仅是物理抗争的武器，而是被赋予了人文关怀与审美价值（仿生学的优雅）。因为技术足以游刃有余地解决问题，所以设计达成了功能与形式的诗意平衡。",
                    datasetIndex: 1
                },
                emotion: {
                    title: "阶段三：后现代的语义重构与精神狂欢",
                    icon: "🛸",
                    desc: "代表案例：Juicy Salif 榨汁机",
                    content: "在物质极大丰富的后现代语境中，面临的不再是“缺乏工具”的危机，而是“意义匮乏”的空虚。因为需要打破常规，所以设计通过刻意制造物理功能上的“失配（Misfit）”，强行制造情感冲击力。此时，形态的反叛与荒诞本身，就构成了设计的最高理由。",
                    datasetIndex: 0
                }
            };

            const textContainer = document.getElementById('dynamic-text-container');
            const tabs = document.querySelectorAll('.interactive-tab');

            function updateStage(stageKey) {
                const data = stageData[stageKey];
                
                // Update Text
                textContainer.style.opacity = 0;
                setTimeout(() => {
                    textContainer.innerHTML = `
                        <div class="text-4xl mb-4">${data.icon}</div>
                        <h4 class="text-xl font-bold text-terracotta mb-2">${data.title}</h4>
                        <p class="text-sm font-bold text-stone-500 mb-4">${data.desc}</p>
                        <p class="text-stone-700 leading-relaxed">${data.content}</p>
                    `;
                    textContainer.style.opacity = 1;
                }, 300);

                // Update Chart
                radarChart.data.datasets.forEach((ds, i) => {
                    if(i === data.datasetIndex) {
                        ds.hidden = false;
                        ds.borderWidth = 4;
                    } else {
                        ds.hidden = true;
                        ds.borderWidth = 2;
                    }
                });
                radarChart.update();

                // Update Button Styles
                tabs.forEach(tab => {
                    if(tab.getAttribute('data-stage') === stageKey) {
                        tab.classList.add('active');
                        tab.classList.remove('text-stone-600', 'bg-transparent');
                    } else {
                        tab.classList.remove('active');
                        tab.classList.add('text-stone-600', 'bg-transparent');
                    }
                });
            }

            // Bind events
            tabs.forEach(tab => {
                tab.addEventListener('click', (e) => {
                    const stage = e.target.getAttribute('data-stage');
                    updateStage(stage);
                });
            });

            // Initialize first stage
            updateStage('survival');
        });
    </script>
</body>
</html>
