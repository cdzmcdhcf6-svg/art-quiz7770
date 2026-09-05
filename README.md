# art-quiz7770
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>名画マスター - 美術品クイズ</title>
<!-- Tailwind CSS CDN -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- FontAwesome for Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700&family=Noto+Serif+JP:wght@500;700&display=swap');
body {
font-family: 'Noto Serif JP', serif;
background-color: #0f172a;
color: #f8fafc;
touch-action: manipulation;
-webkit-tap-highlight-color: transparent;
}
.title-font {
font-family: 'Cinzel', 'Noto Serif JP', serif;
}
/* 額縁風デザイン */
.art-frame {
border: 12px solid #b45309;
border-image: linear-gradient(to bottom right, #fbbf24, #78350f, #d97706, #451a03) 1;
box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.8), 0 10px 10px -5px rgba(0, 0, 0, 0.5);
}
/* アニメーション定義 */
@keyframes pulse-correct {
0% { transform: scale(1); }
50% { transform: scale(1.03); background-color: #16a34a; }
100% { transform: scale(1); }
}
@keyframes shake-wrong {
0%, 100% { transform: translateX(0); }
20%, 60% { transform: translateX(-8px); }
40%, 80% { transform: translateX(8px); }
}
.btn-correct {
animation: pulse-correct 0.4s ease-in-out;
background-color: #16a34a !important;
border-color: #4ade80 !important;
color: white !important;
}
.btn-wrong {
animation: shake-wrong 0.4s ease-in-out;
background-color: #dc2626 !important;
border-color: #f87171 !important;
color: white !important;
}
/* カスタムスクロールバー */
::-webkit-scrollbar {
width: 6px;
}
::-webkit-scrollbar-track {
background: #1e293b;
}
::-webkit-scrollbar-thumb {
background: #475569;
border-radius: 3px;
}
</style>
</head>
<body class="min-h-screen flex flex-col items-center justify-between p-4 max-w-md mx-auto relative overflow-x-hidden">
<!-- ヘッダーエリア -->
<header class="w-full flex items-center justify-between py-2 px-1 border-b border-amber-900/50 mb-2">
<div class="flex items-center space-x-2">
<i class="fa-solid fa-palette text-amber-500 text-xl"></i>
<h1 class="text-lg font-bold tracking-wider text-amber-200 title-font">名画マスター</h1>
</div>
<div class="flex items-center space-x-4">
<!-- ライフ -->
<div id="lives-container" class="flex space-x-1 text-red-500">
<i class="fa-solid fa-heart"></i>
<i class="fa-solid fa-heart"></i>
<i class="fa-solid fa-heart"></i>
</div>
<!-- スコア -->
<div class="text-right">
<div class="text-[10px] text-slate-400">SCORE</div>
<div id="score-display" class="text-base font-bold text-amber-400">0</div>
</div>
</div>
</header>
<!-- メインコンテンツ領域 -->
<main class="w-full flex-1 flex flex-col items-center justify-start relative">
<!-- スタート画面 -->
<div id="start-screen" class="w-full flex flex-col items-center justify-center my-auto py-6 text-center space-y-6">
<div class="relative">
<div class="w-40 h-40 rounded-full border-4 border-amber-500/40 flex items-center justify-center bg-slate-900 shadow-2xl overflow-hidden p-3">
<svg viewBox="0 0 300 300" class="w-full h-full rounded-full">
<rect width="300" height="300" fill="#0f172a"/>
<circle cx="150" cy="150" r="100" fill="#d97706" opacity="0.3"/>
<path d="M50 220 Q150 120 250 220" stroke="#f59e0b" stroke-width="12" fill="none" stroke-linecap="round"/>
<circle cx="100" cy="110" r="25" fill="#38bdf8"/>
<circle cx="180" cy="90" r="20" fill="#f472b6"/>
<circle cx="210" cy="150" r="18" fill="#4ade80"/>
</svg>
</div>
<div class="absolute -bottom-2 -right-2 bg-amber-600 text-slate-950 p-3 rounded-full shadow-lg">
<i class="fa-solid fa-graduation-cap text-xl"></i>
</div>
</div>
<div class="space-y-2">
<h2 class="text-2xl font-bold text-amber-100">歴史的名画クイズ</h2>
<p class="text-xs text-slate-300 px-4 leading-relaxed">
アートイラストを見て、
<span class="text-amber-400 font-semibold">「作品名」</span>と<span class="text-amber-400 font-semibold">「作者」</span>を当てよう！
全問正解して名画マスターを目指せ！
</p>
</div>
<button id="start-btn" class="w-4/5 py-3.5 bg-gradient-to-r from-amber-600 to-amber-500 hover:from-amber-500 hover:to-amber-400 text-slate-950 font-bold text-lg rounded-xl shadow-lg shadow-amber-900/40 transform active:scale-95 transition flex items-center justify-center space-x-2">
<i class="fa-solid fa-play"></i>
<span>ゲームスタート</span>
</button>
<div class="text-xs text-slate-500 pt-2">
最高記録: <span id="high-score-display" class="text-slate-300 font-bold">0</span> pt
</div>
</div>
<!-- クイズ画面 -->
<div id="quiz-screen" class="w-full flex flex-col items-center space-y-3 hidden">
<!-- プログレスバー & コンボ -->
<div class="w-full flex items-center justify-between text-xs text-slate-400 px-1">
<span id="question-count">Q. 1 / 10</span>
<span id="combo-badge" class="bg-gradient-to-r from-amber-500 to-red-500 text-slate-950 px-2 py-0.5 rounded-full font-bold opacity-0 transition-opacity">
🔥 <span id="combo-count">2</span> COMBO!
</span>
</div>
<!-- アートSVG表示エリア（額縁） -->
<div class="w-full flex justify-center my-1">
<div class="relative w-64 h-64 bg-slate-950 flex items-center justify-center overflow-hidden art-frame rounded-sm">
<div id="art-svg-container" class="w-full h-full flex items-center justify-center">
</div>
</div>
</div>
<!-- 質問タイトル -->
<div class="w-full bg-slate-800/90 border border-slate-700/60 rounded-lg p-2.5 text-center shadow-md">
<span id="step-badge" class="inline-block bg-amber-500/20 text-amber-300 border border-amber-500/40 text-[11px] px-2 py-0.5 rounded mb-1">STEP 1/2</span>
<h3 id="question-text" class="text-sm font-bold text-slate-100">この作品の【作品名】は何でしょう？</h3>
</div>
<!-- 選択肢ボタン（4択） -->
<div id="options-container" class="w-full grid grid-cols-1 gap-2">
</div>
</div>
<!-- 結果/解説モーダル（問題ごと） -->
<div id="explanation-modal" class="fixed inset-0 bg-slate-950/85 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden">
<div class="bg-slate-900 border border-amber-600/40 w-full max-w-sm rounded-2xl p-5 shadow-2xl flex flex-col items-center text-center space-y-3">
<div id="result-icon-container" class="w-12 h-12 rounded-full flex items-center justify-center text-xl font-bold">
</div>
<div class="space-y-0.5">
<h4 id="result-title" class="text-lg font-bold">正解！</h4>
<p id="result-score-add" class="text-xs text-amber-400 font-semibold">+100 pt</p>
</div>
<div class="w-full bg-slate-800/90 rounded-xl p-3 text-left space-y-2 border border-slate-700/50">
<div class="border-b border-slate-700/80 pb-1.5">
<div class="text-[10px] text-amber-400 font-semibold">作品名</div>
<div id="exp-title" class="font-bold text-slate-100 text-sm">モナ・リザ</div>
</div>
<div class="border-b border-slate-700/80 pb-1.5">
<div class="text-[10px] text-amber-400 font-semibold">作者 (制作年)</div>
<div id="exp-artist" class="font-bold text-slate-100 text-sm">レオナルド・ダ・ヴィンチ (1503年頃)</div>
</div>
<div>
<div class="text-[10px] text-amber-400 font-semibold">解説・豆知識</div>
<p id="exp-description" class="text-xs text-slate-300 leading-relaxed mt-1">
ルーヴル美術館が所蔵する、世界で最も有名な肖像画の一つ。
</p>
</div>
</div>
<button id="next-btn" class="w-full py-3 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold rounded-xl shadow-md transition active:scale-95 text-sm">
次の問題へ
</button>
</div>
</div>
<!-- ゲームオーバー / 最終結果画面 -->
<div id="gameover-screen" class="w-full flex flex-col items-center justify-center my-auto py-6 text-center space-y-5 hidden">
<div class="relative">
<i class="fa-solid fa-trophy text-6xl text-amber-400 drop-shadow-[0_10px_10px_rgba(245,158,11,0.3)]"></i>
</div>
<div class="space-y-1">
<h2 id="gameover-title" class="text-2xl font-bold text-slate-100">ゲーム終了！</h2>
<p id="gameover-subtitle" class="text-xs text-slate-400">お疲れ様でした！あなたの美術力は...</p>
</div>
<!-- スコアカード -->
<div class="w-full bg-slate-800/80 border border-amber-600/30 rounded-2xl p-4 space-y-3">
<div class="flex justify-between items-center border-b border-slate-700 pb-2">
<span class="text-xs text-slate-400">最終スコア</span>
<span id="final-score" class="text-xl font-bold text-amber-400">0 pt</span>
</div>
<div class="flex justify-between items-center border-b border-slate-700 pb-2">
<span class="text-xs text-slate-400">正解問題数</span>
<span id="final-correct" class="text-sm font-semibold text-slate-200">0 / 10</span>
</div>
<div class="flex justify-between items-center">
<span class="text-xs text-slate-400">称号</span>
<span id="final-rank" class="text-xs font-bold text-amber-300 bg-amber-950/60 border border-amber-500/40 px-2.5 py-1 rounded-full">美術初心者</span>
</div>
</div>
<button id="restart-btn" class="w-4/5 py-3.5 bg-gradient-to-r from-amber-600 to-amber-500 hover:from-amber-500 text-slate-950 font-bold text-base rounded-xl shadow-lg active:scale-95 transition flex items-center justify-center space-x-2">
<i class="fa-solid fa-rotate-right"></i>
<span>もう一度遊ぶ</span>
</button>
</div>
</main>
<!-- フッター -->
<footer class="w-full text-center py-1 text-[10px] text-slate-500">
© 名画マスター - Fine Art Quiz Game
</footer>
<!-- JavaScript ロジック -->
<script>
const ARTWORKS = [
{
id: 'mona_lisa',
title: 'モナ・リザ',
artist: 'レオナルド・ダ・ヴィンチ',
year: '1503年頃',
description: 'ルーヴル美術館所蔵。世界で最も有名な肖像画。輪郭線をぼかす「スフマート技法」で神秘的な微笑みを生み出しています。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><defs><radialGradient id="ml-bg" cx="50%" cy="50%" r="50%"><stop offset="0%" stop-color="#4a3b2c"/><stop offset="100%" stop-color="#1f160e"/></radialGradient><linearGradient id="ml-skin" x1="0%" y1="0%" x2="0%" y2="100%"><stop offset="0%" stop-color="#e8c39e"/><stop offset="100%" stop-color="#c99a6e"/></linearGradient></defs><rect width="300" height="300" fill="url(#ml-bg)"/><path d="M0 200 Q75 180 150 200 T300 190 L300 300 L0 300 Z" fill="#2d3a27" opacity="0.6"/><path d="M90 110 C80 50 220 50 210 110 C220 190 210 270 210 300 L90 300 C90 270 80 190 90 110 Z" fill="#1a120b"/><ellipse cx="150" cy="115" rx="38" ry="48" fill="url(#ml-skin)"/><ellipse cx="136" cy="110" rx="5" ry="3" fill="#3a2312"/><ellipse cx="164" cy="110" rx="5" ry="3" fill="#3a2312"/><path d="M140 135 Q150 142 160 135" stroke="#7a4220" stroke-width="2.5" fill="none" stroke-linecap="round"/><path d="M100 155 Q150 175 200 155 L220 300 L80 300 Z" fill="#3d311d"/><path d="M120 155 Q150 165 180 155 L185 210 L115 210 Z" fill="#7a5c29"/><ellipse cx="150" cy="245" rx="55" ry="22" fill="url(#ml-skin)"/></svg>⁠
},
{
id: 'starry_night',
title: '星月夜',
artist: 'フィンセント・ファン・ゴッホ',
year: '1889年',
description: '精神病院の窓から見た夜空を描いた代表作。渦巻く雲や輝く星々がゴッホの激しい情熱と内面世界を表現しています。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><defs><linearGradient id="sn-bg" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#0b1b3d"/><stop offset="60%" stop-color="#163866"/><stop offset="100%" stop-color="#2a528a"/></linearGradient></defs><rect width="300" height="300" fill="url(#sn-bg)"/><path d="M20 80 Q100 40 170 80 T290 60" stroke="#60a5fa" stroke-width="8" stroke-linecap="round" fill="none" opacity="0.6"/><path d="M10 110 Q90 140 180 90 T280 120" stroke="#fef08a" stroke-width="6" stroke-linecap="round" fill="none" opacity="0.7"/><circle cx="240" cy="60" r="26" fill="#fef08a"/><circle cx="228" cy="54" r="20" fill="#163866"/><circle cx="50" cy="50" r="12" fill="#fef08a" opacity="0.9"/><circle cx="50" cy="50" r="6" fill="#ffffff"/><circle cx="120" cy="40" r="10" fill="#fbbf24" opacity="0.9"/><circle cx="180" cy="70" r="14" fill="#fef08a" opacity="0.9"/><path d="M0 230 L300 230 L300 300 L0 300 Z" fill="#0f172a"/><polygon points="180,210 170,230 190,230" fill="#1e293b"/><polygon points="210,190 205,230 215,230" fill="#334155"/><path d="M20 300 Q50 180 35 80 Q65 160 80 300 Z" fill="#091e11"/><path d="M30 300 Q60 170 45 90 Q70 170 70 300 Z" fill="#143821"/></svg>⁠
},
{
id: 'girl_pearl_earring',
title: '真珠の耳飾りの少女',
artist: 'ヨハネス・フェルメール',
year: '1665年頃',
description: '「北のモナ・リザ」と称されるフェルメールの傑作。暗い背景に鮮やかな青いターバンと光る大粒の真珠が浮かび上がります。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="300" fill="#090d16"/><path d="M100 130 C80 60 210 50 210 110 C210 140 180 150 160 150 Z" fill="#1d4ed8"/><path d="M170 120 C220 140 230 220 220 300 C190 300 170 200 160 150 Z" fill="#eab308"/><ellipse cx="140" cy="140" rx="32" ry="42" fill="#fed7aa"/><circle cx="162" cy="168" r="10" fill="#cbd5e1"/><circle cx="160" cy="166" r="4" fill="#ffffff"/><path d="M90 180 Q140 170 180 200 L210 300 L70 300 Z" fill="#854d0e"/><path d="M130 175 L150 190 L135 195 Z" fill="#ffffff"/></svg>⁠
},
{
id: 'the_scream',
title: '叫び',
artist: 'エドヴァルド・ムンク',
year: '1893年',
description: '自然を突き抜ける巨大な「叫び」に耳を塞ぐ人物を描いた表現主義の代表作。人物自身が叫んでいるわけではありません。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="300" fill="#451a03"/><path d="M0 20 Q150 60 300 20 L300 0 L0 0 Z" fill="#ea580c"/><path d="M0 50 Q150 10 300 60 L300 20 L0 20 Z" fill="#f97316"/><path d="M0 90 Q150 130 300 80 L300 50 L0 50 Z" fill="#fde047"/><path d="M0 110 Q150 160 300 120 L300 220 L0 200 Z" fill="#1e3a8a"/><polygon points="0,170 300,280 300,300 0,220" fill="#78350f"/><line x1="0" y1="180" x2="300" y2="290" stroke="#b45309" stroke-width="4"/><path d="M120 300 C120 240 140 220 150 200 C160 220 180 240 180 300 Z" fill="#0f172a"/><path d="M125 180 Q130 150 140 150" stroke="#fef08a" stroke-width="8" stroke-linecap="round" fill="none"/><path d="M175 180 Q170 150 160 150" stroke="#fef08a" stroke-width="8" stroke-linecap="round" fill="none"/><ellipse cx="150" cy="150" rx="20" ry="28" fill="#fef08a"/><ellipse cx="142" cy="145" rx="4" ry="6" fill="#020617"/><ellipse cx="158" cy="145" rx="4" ry="6" fill="#020617"/><ellipse cx="150" cy="165" rx="6" ry="10" fill="#020617"/></svg>⁠
},
{
id: 'great_wave',
title: '神奈川沖浪裏',
artist: '葛飾北斎',
year: '1831年頃',
description: '「富嶽三十六景」の代表作。大きくダイナミックにうねる大波と遠景の静かな富士山のコントラストが世界的に超有名です。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="300" fill="#fef3c7"/><polygon points="150,180 180,240 120,240" fill="#1e293b"/><polygon points="150,180 160,200 140,200" fill="#ffffff"/><path d="M0 210 Q120 150 250 220 L300 300 L0 300 Z" fill="#1e40af" opacity="0.7"/><path d="M0 300 Q60 260 100 200 Q150 120 220 80 Q190 100 160 130 Q120 170 90 220 Q50 250 0 260 Z" fill="#1e3a8a"/><path d="M0 300 Q80 220 150 100 Q180 60 210 70 Q180 85 150 120 Q100 180 0 240 Z" fill="#0284c7"/><path d="M210 70 Q200 60 190 75 Q180 50 170 80 Q160 65 150 90 Q130 80 120 110" fill="none" stroke="#ffffff" stroke-width="5" stroke-linecap="round"/><circle cx="215" cy="72" r="4" fill="#ffffff"/><circle cx="195" cy="62" r="3" fill="#ffffff"/><circle cx="175" cy="55" r="4" fill="#ffffff"/><path d="M60 240 Q110 245 160 235 Q110 250 60 240 Z" fill="#d97706"/></svg>⁠
},
{
id: 'birth_of_venus',
title: 'ヴィーナスの誕生',
artist: 'サンドロ・ボッティチェッリ',
year: '1485年頃',
description: 'ギリシア神話の愛と美の女神ヴィーナスが、成熟した女性として貝殻に乗って海から出現する場面を描いた傑作。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="180" fill="#bae6fd"/><rect y="180" width="300" height="120" fill="#0284c7"/><path d="M0 200 Q30 195 60 200 T120 200 T180 200 T240 200 T300 200" stroke="#38bdf8" stroke-width="2" fill="none"/><path d="M80 260 Q150 200 220 260 Q150 290 80 260 Z" fill="#fde047" stroke="#ca8a04" stroke-width="3"/><ellipse cx="150" cy="110" rx="16" ry="22" fill="#fed7aa"/><path d="M135 100 Q110 140 120 210 Q140 180 145 130 Z" fill="#eab308"/><path d="M165 100 Q190 140 180 200 Q160 170 155 130 Z" fill="#ca8a04"/><path d="M142 130 Q135 170 145 230 L155 230 Q165 170 158 130 Z" fill="#fed7aa"/><circle cx="60" cy="80" r="5" fill="#f472b6"/><circle cx="230" cy="90" r="6" fill="#f472b6"/></svg>⁠
},
{
id: 'last_supper',
title: '最後の晩餐',
artist: 'レオナルド・ダ・ヴィンチ',
year: '1498年',
description: 'キリストが「弟子の中に裏切り者がいる」と予告した瞬間の動揺を描いたミラノの修道院にある巨大な壁画。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="300" fill="#451a03"/><polygon points="0,0 60,60 60,240 0,300" fill="#78350f"/><polygon points="300,0 240,60 240,240 300,300" fill="#78350f"/><rect x="100" y="50" width="30" height="60" fill="#38bdf8"/><rect x="135" y="45" width="30" height="65" fill="#7dd3fc"/><rect x="170" y="50" width="30" height="60" fill="#38bdf8"/><polygon points="30,170 270,170 290,260 10,260" fill="#f8fafc"/><circle cx="150" cy="130" r="18" fill="#fef08a" opacity="0.5"/><ellipse cx="150" cy="130" rx="10" ry="14" fill="#fed7aa"/><path d="M135 144 L165 144 L170 170 L130 170 Z" fill="#dc2626"/><circle cx="70" cy="140" r="8" fill="#fed7aa"/><circle cx="90" cy="135" r="8" fill="#fed7aa"/><circle cx="110" cy="140" r="8" fill="#fed7aa"/><path d="M50 170 C50 150 120 150 125 170 Z" fill="#2563eb"/><circle cx="190" cy="140" r="8" fill="#fed7aa"/><circle cx="210" cy="135" r="8" fill="#fed7aa"/><circle cx="230" cy="140" r="8" fill="#fed7aa"/><path d="M175 170 C180 150 250 150 250 170 Z" fill="#16a34a"/></svg>⁠
},
{
id: 'water_lilies',
title: '睡蓮',
artist: 'クロード・モネ',
year: '1916年頃',
description: '自宅庭園の池と睡蓮を描いた印象派の金字塔。光の移り変わりや水面の反射が見事に美しく表現されています。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><defs><linearGradient id="wl-bg" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#064e3b"/><stop offset="50%" stop-color="#0f766e"/><stop offset="100%" stop-color="#1e3a8a"/></linearGradient></defs><rect width="300" height="300" fill="url(#wl-bg)"/><ellipse cx="150" cy="80" rx="120" ry="40" fill="#a7f3d0" opacity="0.2"/><ellipse cx="70" cy="120" rx="40" ry="18" fill="#15803d"/><ellipse cx="220" cy="90" rx="50" ry="20" fill="#166534"/><ellipse cx="140" cy="200" rx="60" ry="22" fill="#14532d"/><circle cx="75" cy="115" r="10" fill="#f472b6"/><circle cx="75" cy="115" r="5" fill="#ffffff"/><circle cx="210" cy="85" r="14" fill="#fb7185"/><circle cx="210" cy="85" r="7" fill="#fef08a"/><circle cx="150" cy="195" r="12" fill="#e879f9"/><circle cx="150" cy="195" r="6" fill="#ffffff"/></svg>⁠
},
{
id: 'guernica',
title: 'ゲルニカ',
artist: 'パブロ・ピカソ',
year: '1937年',
description: '無差別爆撃に対する抗議として描かれたモノクロームのキュビスムの巨編。戦争の無差別な悲劇を表現しています。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><rect width="300" height="300" fill="#18181b"/><polygon points="150,20 130,60 170,60" fill="#f4f4f5"/><circle cx="150" cy="65" r="18" fill="#ffffff"/><polygon points="120,130 180,110 160,160 110,150" fill="#e4e4e7"/><polygon points="180,110 220,80 210,120" fill="#a1a1aa"/><polygon points="30,240 120,270 90,280 20,250" fill="#71717a"/><line x1="40" y1="230" x2="100" y2="270" stroke="#ffffff" stroke-width="4"/><polygon points="30,120 70,100 80,150 40,160" fill="#d4d4d8"/><circle cx="50" cy="90" r="15" fill="#f4f4f5"/><polygon points="220,160 280,140 270,220 210,200" fill="#a1a1aa"/></svg>⁠
},
{
id: 'the_kiss',
title: '接吻',
artist: 'グスタフ・クリムト',
year: '1908年',
description: '「金箔時代」の最高峰。絢爛豪華な金箔模様背景の中で、崖の端で抱き合う恋人たちを神秘的に描いています。',
svg: ⁠<svg viewBox="0 0 300 300" class="w-full h-full"><defs><radialGradient id="kiss-bg" cx="50%" cy="50%" r="50%"><stop offset="0%" stop-color="#fef08a"/><stop offset="60%" stop-color="#eab308"/><stop offset="100%" stop-color="#854d0e"/></radialGradient></defs><rect width="300" height="300" fill="url(#kiss-bg)"/><circle cx="50" cy="50" r="3" fill="#ffffff"/><circle cx="250" cy="80" r="4" fill="#ffffff"/><ellipse cx="150" cy="260" rx="100" ry="30" fill="#15803d"/><circle cx="100" cy="250" r="4" fill="#f472b6"/><circle cx="170" cy="255" r="4" fill="#c084fc"/><path d="M100 130 C100 70 200 70 200 130 C210 200 190 260 150 260 C110 260 90 200 100 130 Z" fill="#ca8a04" stroke="#78350f" stroke-width="3"/><rect x="115" y="140" width="12" height="24" fill="#090d16"/><rect x="130" y="160" width="14" height="20" fill="#ffffff"/><circle cx="170" cy="150" r="8" fill="#ec4899"/><circle cx="165" cy="180" r="10" fill="#06b6d4"/><circle cx="140" cy="100" r="14" fill="#fed7aa"/><circle cx="160" cy="105" r="13" fill="#fed7aa"/></svg>⁠
}
];
const DUMMY_ARTISTS = ['ミケランジェロ', 'ラファエロ', 'エドゥアール・マネ', 'オーギュスト・ルノワール', 'ポール・セザンヌ', 'サルバドール・ダリ', 'マルク・シャガール', 'アンリ・マティス', '歌川広重', '尾形光琳'];
const DUMMY_TITLES = ['日傘を差す女', '民衆を導く自由の女神', 'アテナイの学堂', '記憶の固執', 'グランド・ジャット島の日曜日の午後', '夜警', '真珠の首飾りの少女', '落穂拾い', 'アヴィニョンの娘たち', '風神雷神図'];
let currentQuestions = [];
let currentIndex = 0;
let currentStep = 1;
let score = 0;
let combo = 0;
let lives = 3;
let correctAnswersCount = 0;
let highScore = parseInt(localStorage.getItem('art_quiz_highscore') || '0');
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
function playSound(type) {
try {
if (audioCtx.state === 'suspended') audioCtx.resume();
const osc = audioCtx.createOscillator();
const gain = audioCtx.createGain();
osc.connect(gain);
gain.connect(audioCtx.destination);
const now = audioCtx.currentTime;
if (type === 'correct') {
osc.type = 'sine';
osc.frequency.setValueAtTime(523.25, now);
osc.frequency.setValueAtTime(659.25, now + 0.1);
osc.frequency.setValueAtTime(783.99, now + 0.2);
gain.gain.setValueAtTime(0.2, now);
gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
osc.start(now);
osc.stop(now + 0.4);
} else if (type === 'wrong') {
osc.type = 'sawtooth';
osc.frequency.setValueAtTime(220, now);
osc.frequency.setValueAtTime(164.81, now + 0.15);
gain.gain.setValueAtTime(0.3, now);
gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
osc.start(now);
osc.stop(now + 0.4);
} else if (type === 'combo') {
osc.type = 'triangle';
osc.frequency.setValueAtTime(587.33, now);
osc.frequency.setValueAtTime(880, now + 0.1);
gain.gain.setValueAtTime(0.2, now);
gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
osc.start(now);
osc.stop(now + 0.3);
}
} catch (e) {}
}
const startScreen = document.getElementById('start-screen');
const quizScreen = document.getElementById('quiz-screen');
const gameoverScreen = document.getElementById('gameover-screen');
const explanationModal = document.getElementById('explanation-modal');
const startBtn = document.getElementById('start-btn');
const restartBtn = document.getElementById('restart-btn');
const nextBtn = document.getElementById('next-btn');
const scoreDisplay = document.getElementById('score-display');
const highScoreDisplay = document.getElementById('high-score-display');
const livesContainer = document.getElementById('lives-container');
const comboBadge = document.getElementById('combo-badge');
const comboCount = document.getElementById('combo-count');
const questionCount = document.getElementById('question-count');
const stepBadge = document.getElementById('step-badge');
const questionText = document.getElementById('question-text');
const artSvgContainer = document.getElementById('art-svg-container');
const optionsContainer = document.getElementById('options-container');
highScoreDisplay.textContent = highScore;
startBtn.addEventListener('click', startGame);
restartBtn.addEventListener('click', startGame);
nextBtn.addEventListener('click', goToNextQuestion);
function shuffle(array) {
const arr = [...array];
for (let i = arr.length - 1; i > 0; i--) {
const j = Math.floor(Math.random() * (i + 1));
[arr[i], arr[j]] = [arr[j], arr[i]];
}
return arr;
}
function startGame() {
score = 0;
combo = 0;
lives = 3;
currentIndex = 0;
correctAnswersCount = 0;
currentQuestions = shuffle(ARTWORKS);
updateHeader();
startScreen.classList.add('hidden');
gameoverScreen.classList.add('hidden');
quizScreen.classList.remove('hidden');
loadQuestion();
}
function updateHeader() {
scoreDisplay.textContent = score;
livesContainer.innerHTML = '';
for (let i = 0; i < 3; i++) {
const heart = document.createElement('i');
heart.className = i < lives ? 'fa-solid fa-heart text-red-500' : 'fa-regular fa-heart text-slate-600';
livesContainer.appendChild(heart);
}
if (combo >= 2) {
comboCount.textContent = combo;
comboBadge.classList.remove('opacity-0');
} else {
comboBadge.classList.add('opacity-0');
}
}
function loadQuestion() {
const item = currentQuestions[currentIndex];
currentStep = 1;
questionCount.textContent = ⁠Q. ${currentIndex + 1} / ${currentQuestions.length}⁠;
artSvgContainer.innerHTML = item.svg;
renderStep();
}
function renderStep() {
const item = currentQuestions[currentIndex];
optionsContainer.innerHTML = '';
if (currentStep === 1) {
stepBadge.textContent = 'STEP 1/2';
questionText.textContent = 'この絵画の【作品名】は何でしょう？';
const wrongOptions = shuffle(ARTWORKS.filter(a => a.id !== item.id).map(a => a.title).concat(DUMMY_TITLES)).slice(0, 3);
const choices = shuffle([item.title, ...wrongOptions]);
renderOptions(choices, item.title);
} else {
stepBadge.textContent = 'STEP 2/2';
questionText.textContent = 'この絵画を描いた【作者】は誰でしょう？';
const wrongOptions = shuffle(ARTWORKS.filter(a => a.artist !== item.artist).map(a => a.artist).concat(DUMMY_ARTISTS)).slice(0, 3);
const choices = shuffle([item.artist, ...wrongOptions]);
renderOptions(choices, item.artist);
}
}
function renderOptions(choices, correctAnswer) {
choices.forEach((choice, index) => {
const btn = document.createElement('button');
btn.className = 'w-full py-3 px-3.5 bg-slate-800 hover:bg-slate-700 active:bg-slate-600 border border-slate-700/80 rounded-xl text-left font-semibold text-slate-100 flex items-center justify-between transition-all shadow-md';
const label = ['A', 'B', 'C', 'D'][index];
btn.innerHTML = ⁠<div class="flex items-center space-x-3"> <span class="w-5 h-5 rounded-full bg-slate-700 text-amber-400 border border-slate-600 flex items-center justify-center text-[10px] font-bold">${label}</span> <span class="text-xs sm:text-sm">${choice}</span> </div> <i class="fa-solid fa-chevron-right text-[10px] text-slate-500"></i>⁠;
btn.addEventListener('click', () => handleAnswer(btn, choice, correctAnswer));
optionsContainer.appendChild(btn);
});
}
function handleAnswer(selectedBtn, selectedChoice, correctAnswer) {
const allBtns = optionsContainer.querySelectorAll('button');
allBtns.forEach(b => b.style.pointerEvents = 'none');
const isCorrect = (selectedChoice === correctAnswer);
if (isCorrect) {
selectedBtn.classList.add('btn-correct');
playSound('correct');
if (currentStep === 1) {
setTimeout(() => {
currentStep = 2;
renderStep();
}, 600);
} else {
combo++;
const bonusScore = 100 + (combo > 1 ? (combo - 1) * 20 : 0);
score += bonusScore;
correctAnswersCount++;
if (combo > 1) playSound('combo');
updateHeader();
setTimeout(() => showExplanation(true, bonusScore), 600);
}
} else {
selectedBtn.classList.add('btn-wrong');
playSound('wrong');
allBtns.forEach(btn => {
if (btn.textContent.includes(correctAnswer)) btn.classList.add('btn-correct');
});
combo = 0;
lives--;
updateHeader();
setTimeout(() => showExplanation(false, 0), 800);
}
}
function showExplanation(isFullyCorrect, earnedPoints) {
const item = currentQuestions[currentIndex];
const iconContainer = document.getElementById('result-icon-container');
const resultTitle = document.getElementById('result-title');
const resultScoreAdd = document.getElementById('result-score-add');
if (isFullyCorrect) {
iconContainer.className = 'w-12 h-12 rounded-full flex items-center justify-center text-xl font-bold bg-green-500/20 text-green-400 border border-green-500/40';
iconContainer.innerHTML = '<i class="fa-solid fa-check"></i>';
resultTitle.textContent = '完全正解！';
resultTitle.className = 'text-lg font-bold text-green-400';
resultScoreAdd.textContent = ⁠+${earnedPoints} pt ${combo > 1 ? ⁠(${combo} COMBO!)⁠ : ''}⁠;
resultScoreAdd.classList.remove('hidden');
} else {
iconContainer.className = 'w-12 h-12 rounded-full flex items-center justify-center text-xl font-bold bg-red-500/20 text-red-400 border border-red-500/40';
iconContainer.innerHTML = '<i class="fa-solid fa-xmark"></i>';
resultTitle.textContent = '不正解...';
resultTitle.className = 'text-lg font-bold text-red-400';
resultScoreAdd.classList.add('hidden');
}
document.getElementById('exp-title').textContent = item.title;
document.getElementById('exp-artist').textContent = ⁠${item.artist} (${item.year})⁠;
document.getElementById('exp-description').textContent = item.description;
explanationModal.classList.remove('hidden');
}
function goToNextQuestion() {
explanationModal.classList.add('hidden');
currentIndex++;
if (lives <= 0 || currentIndex >= currentQuestions.length) {
endGame();
} else {
loadQuestion();
}
}
function endGame() {
quizScreen.classList.add('hidden');
gameoverScreen.classList.remove('hidden');
if (score > highScore) {
highScore = score;
localStorage.setItem('art_quiz_highscore', highScore.toString());
highScoreDisplay.textContent = highScore;
}
document.getElementById('final-score').textContent = ⁠${score} pt⁠;
document.getElementById('final-correct').textContent = ⁠${correctAnswersCount} / ${currentQuestions.length}⁠;
const rankBadge = document.getElementById('final-rank');
if (score >= 1200) {
rankBadge.textContent = '👑 ルーヴル館長級 (伝説)';
} else if (score >= 800) {
rankBadge.textContent = '🎨 美術鑑定士';
} else if (score >= 500) {
rankBadge.textContent = '🖼️ アート愛好家';
} else {
rankBadge.textContent = '🌱 美術初心者';
}
if (lives <= 0) {
document.getElementById('gameover-title').textContent = 'ライフがなくなりました';
} else {
document.getElementById('gameover-title').textContent = '全問クリア！';
}
}
</script>
</body>
</html>