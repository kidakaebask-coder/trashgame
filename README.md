<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Trash Separation Game</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Comic Sans MS', 'Arial', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      height: 100%;
      overflow: hidden;
    }

    html {
      height: 100%;
    }

    .game-container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
      height: 100%;
      display: flex;
      flex-direction: column;
    }

    .header {
      text-align: center;
      margin-bottom: 20px;
    }

    .game-title {
      font-size: 48px;
      color: white;
      margin: 0 0 10px 0;
      text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
    }

    .score-container {
      background: white;
      padding: 15px 30px;
      border-radius: 25px;
      display: inline-block;
      box-shadow: 0 4px 6px rgba(0,0,0,0.2);
    }

    .score {
      font-size: 24px;
      font-weight: bold;
      color: #667eea;
    }

    .instructions {
      background: rgba(255,255,255,0.9);
      padding: 15px;
      border-radius: 15px;
      text-align: center;
      font-size: 18px;
      margin-bottom: 20px;
      color: #333;
    }

    .game-area {
      flex: 1;
      display: flex;
      gap: 20px;
      min-height: 0;
    }

    .items-area {
      flex: 1;
      background: rgba(255,255,255,0.9);
      border-radius: 20px;
      padding: 20px;
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      align-content: flex-start;
      overflow-y: auto;
    }

    .trash-item {
      width: 80px;
      height: 80px;
      background: white;
      border-radius: 15px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      cursor: grab;
      box-shadow: 0 2px 8px rgba(0,0,0,0.15);
      transition: transform 0.2s;
      user-select: none;
    }

    .trash-item:hover {
      transform: scale(1.05);
    }

    .trash-item.dragging {
      opacity: 0.5;
      cursor: grabbing;
    }

    .item-icon {
      font-size: 36px;
      margin-bottom: 5px;
    }

    .item-name {
      font-size: 11px;
      color: #666;
      text-align: center;
    }

    .bins-area {
      width: 280px;
      display: flex;
      flex-direction: column;
      gap: 15px;
    }

    .bin {
      flex: 1;
      border-radius: 20px;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      transition: all 0.3s;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
      position: relative;
    }

    .bin.drag-over {
      transform: scale(1.05);
      box-shadow: 0 6px 20px rgba(0,0,0,0.3);
    }

    .bin-recycle {
      background: linear-gradient(135deg, #4CAF50, #66BB6A);
    }

    .bin-general {
      background: linear-gradient(135deg, #757575, #9E9E9E);
    }

    .bin-compost {
      background: linear-gradient(135deg, #8D6E63, #A1887F);
    }

    .bin-icon {
      font-size: 48px;
      margin-bottom: 10px;
    }

    .bin-label {
      font-size: 20px;
      font-weight: bold;
      color: white;
      text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
    }

    .feedback {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 30px 50px;
      border-radius: 20px;
      font-size: 32px;
      font-weight: bold;
      box-shadow: 0 8px 20px rgba(0,0,0,0.3);
      z-index: 1000;
      display: none;
    }

    .feedback.correct {
      color: #4CAF50;
      display: block;
      animation: fadeInOut 1s;
    }

    .feedback.incorrect {
      color: #f44336;
      display: block;
      animation: shake 0.5s;
    }

    @keyframes fadeInOut {
      0%, 100% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
      50% { opacity: 1; transform: translate(-50%, -50%) scale(1.1); }
    }

    @keyframes shake {
      0%, 100% { transform: translate(-50%, -50%); }
      25% { transform: translate(-45%, -50%); }
      75% { transform: translate(-55%, -50%); }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="game-container">
   <div class="header">
    <h1 class="game-title" id="gameTitle">♻️ Trash Separation Game</h1>
    <div class="score-container"><span class="score" id="scoreLabel">Score:</span> <span class="score" id="scoreValue">0</span>
    </div>
   </div>
   <div class="instructions" id="instructions">
    Drag each item to the correct bin! Recycle ♻️, General 🗑️, or Compost 🌱
   </div>
   <div class="game-area">
    <div class="items-area" id="itemsArea"></div>
    <div class="bins-area">
     <div class="bin bin-recycle" data-type="recycle">
      <div class="bin-icon">
       ♻️
      </div>
      <div class="bin-label">
       Recycle
      </div>
     </div>
     <div class="bin bin-general" data-type="general">
      <div class="bin-icon">
       🗑️
      </div>
      <div class="bin-label">
       General
      </div>
     </div>
     <div class="bin bin-compost" data-type="compost">
      <div class="bin-icon">
       🌱
      </div>
      <div class="bin-label">
       Compost
      </div>
     </div>
    </div>
   </div>
   <div class="feedback" id="feedback"></div>
  </div>
  <script>
    const defaultConfig = {
      game_title: "♻️ Trash Separation Game",
      score_label: "Score:",
      instructions_text: "Drag each item to the correct bin! Recycle ♻️, General 🗑️, or Compost 🌱",
      primary_color: "#667eea",
      background_color: "#764ba2",
      recycle_color: "#4CAF50",
      general_color: "#757575",
      compost_color: "#8D6E63",
      font_size: 16
    };

    let score = 0;
    let draggedItem = null;

    const trashItems = [
      { icon: "📰", name: "Newspaper", type: "recycle" },
      { icon: "🥫", name: "Can", type: "recycle" },
      { icon: "🍾", name: "Bottle", type: "recycle" },
      { icon: "📦", name: "Cardboard", type: "recycle" },
      { icon: "🍌", name: "Banana", type: "compost" },
      { icon: "🍎", name: "Apple", type: "compost" },
      { icon: "🥬", name: "Lettuce", type: "compost" },
      { icon: "🍕", name: "Pizza Box", type: "compost" },
      { icon: "💼", name: "Bag", type: "general" },
      { icon: "🧴", name: "Shampoo", type: "general" },
      { icon: "🔋", name: "Battery", type: "general" },
      { icon: "💡", name: "Light Bulb", type: "general" }
    ];

    function initGame() {
      const itemsArea = document.getElementById('itemsArea');
      itemsArea.innerHTML = '';
      
      const shuffled = [...trashItems].sort(() => Math.random() - 0.5);
      
      shuffled.forEach((item, index) => {
        const itemDiv = document.createElement('div');
        itemDiv.className = 'trash-item';
        itemDiv.draggable = true;
        itemDiv.dataset.type = item.type;
        itemDiv.dataset.index = index;
        itemDiv.innerHTML = `
          <div class="item-icon">${item.icon}</div>
          <div class="item-name">${item.name}</div>
        `;
        
        itemDiv.addEventListener('dragstart', handleDragStart);
        itemDiv.addEventListener('dragend', handleDragEnd);
        
        itemsArea.appendChild(itemDiv);
      });
    }

    function handleDragStart(e) {
      draggedItem = e.target;
      e.target.classList.add('dragging');
      e.dataTransfer.effectAllowed = 'move';
    }

    function handleDragEnd(e) {
      e.target.classList.remove('dragging');
    }

    function handleDragOver(e) {
      e.preventDefault();
      e.dataTransfer.dropEffect = 'move';
      return false;
    }

    function handleDragEnter(e) {
      if (e.target.classList.contains('bin')) {
        e.target.classList.add('drag-over');
      } else if (e.target.closest('.bin')) {
        e.target.closest('.bin').classList.add('drag-over');
      }
    }

    function handleDragLeave(e) {
      if (e.target.classList.contains('bin')) {
        e.target.classList.remove('drag-over');
      } else if (e.target.closest('.bin')) {
        e.target.closest('.bin').classList.remove('drag-over');
      }
    }

    function handleDrop(e) {
      e.preventDefault();
      
      const bin = e.target.classList.contains('bin') ? e.target : e.target.closest('.bin');
      if (!bin || !draggedItem) return;
      
      bin.classList.remove('drag-over');
      
      const itemType = draggedItem.dataset.type;
      const binType = bin.dataset.type;
      
      const feedback = document.getElementById('feedback');
      
      if (itemType === binType) {
        feedback.textContent = '✓ Correct!';
        feedback.className = 'feedback correct';
        score += 10;
        draggedItem.remove();
        
        if (document.querySelectorAll('.trash-item').length === 0) {
          setTimeout(() => {
            feedback.textContent = '🎉 You Win!';
            feedback.className = 'feedback correct';
            setTimeout(() => {
              initGame();
              score = 0;
              updateScore();
            }, 2000);
          }, 1000);
        }
      } else {
        feedback.textContent = '✗ Try Again!';
        feedback.className = 'feedback incorrect';
        score = Math.max(0, score - 5);
      }
      
      updateScore();
      
      setTimeout(() => {
        feedback.className = 'feedback';
      }, 1000);
      
      draggedItem = null;
    }

    function updateScore() {
      document.getElementById('scoreValue').textContent = score;
    }

    const bins = document.querySelectorAll('.bin');
    bins.forEach(bin => {
      bin.addEventListener('dragover', handleDragOver);
      bin.addEventListener('dragenter', handleDragEnter);
      bin.addEventListener('dragleave', handleDragLeave);
      bin.addEventListener('drop', handleDrop);
    });

    async function onConfigChange(config) {
      const baseFont = config.font_size || defaultConfig.font_size;
      
      document.getElementById('gameTitle').textContent = config.game_title || defaultConfig.game_title;
      document.getElementById('scoreLabel').textContent = config.score_label || defaultConfig.score_label;
      document.getElementById('instructions').textContent = config.instructions_text || defaultConfig.instructions_text;
      
      document.querySelector('.game-title').style.fontSize = `${baseFont * 3}px`;
      document.querySelector('.score').style.fontSize = `${baseFont * 1.5}px`;
      document.querySelector('.instructions').style.fontSize = `${baseFont * 1.125}px`;
      document.querySelector('.bin-label').style.fontSize = `${baseFont * 1.25}px`;
      
      document.body.style.background = `linear-gradient(135deg, ${config.primary_color || defaultConfig.primary_color} 0%, ${config.background_color || defaultConfig.background_color} 100%)`;
      document.querySelector('.score').style.color = config.primary_color || defaultConfig.primary_color;
      
      document.querySelector('.bin-recycle').style.background = `linear-gradient(135deg, ${config.recycle_color || defaultConfig.recycle_color}, ${adjustBrightness(config.recycle_color || defaultConfig.recycle_color, 20)})`;
      document.querySelector('.bin-general').style.background = `linear-gradient(135deg, ${config.general_color || defaultConfig.general_color}, ${adjustBrightness(config.general_color || defaultConfig.general_color, 20)})`;
      document.querySelector('.bin-compost').style.background = `linear-gradient(135deg, ${config.compost_color || defaultConfig.compost_color}, ${adjustBrightness(config.compost_color || defaultConfig.compost_color, 20)})`;
    }

    function adjustBrightness(color, percent) {
      const num = parseInt(color.replace("#",""), 16);
      const amt = Math.round(2.55 * percent);
      const R = (num >> 16) + amt;
      const G = (num >> 8 & 0x00FF) + amt;
      const B = (num & 0x0000FF) + amt;
      return "#" + (0x1000000 + (R<255?R<1?0:R:255)*0x10000 + (G<255?G<1?0:G:255)*0x100 + (B<255?B<1?0:B:255)).toString(16).slice(1);
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities: (config) => ({
          recolorables: [
            {
              get: () => config.primary_color || defaultConfig.primary_color,
              set: (value) => {
                config.primary_color = value;
                window.elementSdk.setConfig({ primary_color: value });
              }
            },
            {
              get: () => config.background_color || defaultConfig.background_color,
              set: (value) => {
                config.background_color = value;
                window.elementSdk.setConfig({ background_color: value });
              }
            },
            {
              get: () => config.recycle_color || defaultConfig.recycle_color,
              set: (value) => {
                config.recycle_color = value;
                window.elementSdk.setConfig({ recycle_color: value });
              }
            },
            {
              get: () => config.general_color || defaultConfig.general_color,
              set: (value) => {
                config.general_color = value;
                window.elementSdk.setConfig({ general_color: value });
              }
            },
            {
              get: () => config.compost_color || defaultConfig.compost_color,
              set: (value) => {
                config.compost_color = value;
                window.elementSdk.setConfig({ compost_color: value });
              }
            }
          ],
          borderables: [],
          fontEditable: undefined,
          fontSizeable: {
            get: () => config.font_size || defaultConfig.font_size,
            set: (value) => {
              config.font_size = value;
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }),
        mapToEditPanelValues: (config) => new Map([
          ["game_title", config.game_title || defaultConfig.game_title],
          ["score_label", config.score_label || defaultConfig.score_label],
          ["instructions_text", config.instructions_text || defaultConfig.instructions_text]
        ])
      });
    }

    initGame();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a1d7854c3d34d99',t:'MTc2MzY5OTYxOC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
