<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>แบบทดสอบค้นพบตัวเอง</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      box-sizing: border-box;
    }
    
    @import url('https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap');
    
    * {
      font-family: 'Sarabun', sans-serif;
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="w-full min-h-full">
  <div id="app" class="w-full min-h-full"></div>
  <script>
    const defaultConfig = {
      quiz_title: "แบบทดสอบค้นพบตัวเอง",
      quiz_description: "ทำแบบทดสอบเพื่อเข้าใจตัวเองมากขึ้น",
      result_title: "ผลการทดสอบของคุณ",
      background_color: "#f0f4f8",
      card_color: "#ffffff",
      text_color: "#1a202c",
      primary_button_color: "#4f46e5",
      secondary_button_color: "#94a3b8"
    };

    let currentQuestion = 0;
    let answers = {};
    let showingResult = false;

    const questions = [
      {
        id: 1,
        text: "คุณชอบใช้เวลาว่างอย่างไร?",
        options: [
          { id: "a", text: "อยู่บ้านคนเดียว อ่านหนังสือหรือดูหนัง", personality: "introvert" },
          { id: "b", text: "ออกไปพบปะเพื่อนฝูง สังสรรค์", personality: "extrovert" },
          { id: "c", text: "ทำกิจกรรมกลางแจ้ง ออกกำลังกาย", personality: "active" },
          { id: "d", text: "เรียนรู้สิ่งใหม่ๆ พัฒนาทักษะ", personality: "learner" }
        ]
      },
      {
        id: 2,
        text: "เมื่อเจอปัญหา คุณมักจะ?",
        options: [
          { id: "a", text: "วิเคราะห์อย่างรอบคอบก่อนตัดสินใจ", personality: "analytical" },
          { id: "b", text: "ปรึกษาคนอื่นเพื่อหาทางออก", personality: "collaborative" },
          { id: "c", text: "ลงมือทำทันทีและปรับเปลี่ยนไปเรื่อยๆ", personality: "action" },
          { id: "d", text: "หาข้อมูลเพิ่มเติมก่อนแก้ปัญหา", personality: "researcher" }
        ]
      },
      {
        id: 3,
        text: "คุณมักตัดสินใจโดยอาศัย?",
        options: [
          { id: "a", text: "ความรู้สึกและสัญชาตญาณ", personality: "emotional" },
          { id: "b", text: "ข้อมูลและตรรกะ", personality: "logical" },
          { id: "c", text: "ประสบการณ์ที่เคยผ่านมา", personality: "experienced" },
          { id: "d", text: "คำแนะนำจากคนรอบข้าง", personality: "social" }
        ]
      },
      {
        id: 4,
        text: "สิ่งที่สำคัญที่สุดในชีวิตคุณคือ?",
        options: [
          { id: "a", text: "ความสัมพันธ์กับคนที่รัก", personality: "relationship" },
          { id: "b", text: "ความสำเร็จในหน้าที่การงาน", personality: "career" },
          { id: "c", text: "การเติบโตและพัฒนาตนเอง", personality: "growth" },
          { id: "d", text: "ความสุขและความสมดุลในชีวิต", personality: "balance" }
        ]
      },
      {
        id: 5,
        text: "คุณมองตัวเองว่าเป็นคนแบบไหน?",
        options: [
          { id: "a", text: "มีจินตนาการสูง ชอบความคิดสร้างสรรค์", personality: "creative" },
          { id: "b", text: "เป็นระเบียบ วางแผนทุกอย่าง", personality: "organized" },
          { id: "c", text: "ยืดหยุ่น ปรับตัวได้ง่าย", personality: "flexible" },
          { id: "d", text: "มั่นคง เชื่อมั่นในตัวเอง", personality: "confident" }
        ]
      },
      {
        id: 6,
        text: "งานอดิเรกที่คุณสนใจคือ?",
        options: [
          { id: "a", text: "งานศิลปะ ดนตรี การเขียน", personality: "artistic" },
          { id: "b", text: "กีฬา การออกกำลังกาย", personality: "athletic" },
          { id: "c", text: "เทคโนโลยี วิทยาศาสตร์", personality: "technical" },
          { id: "d", text: "กิจกรรมสังคม อาสาสมัคร", personality: "social" }
        ]
      },
      {
        id: 7,
        text: "ถ้าได้รับคำชม คุณรู้สึก?",
        options: [
          { id: "a", text: "ดีใจมาก และมีกำลังใจมากขึ้น", personality: "motivated" },
          { id: "b", text: "ขอบคุณแต่ไม่ได้คิดมาก", personality: "humble" },
          { id: "c", text: "ภูมิใจในตัวเองและทำต่อไป", personality: "proud" },
          { id: "d", text: "กังวลว่าจะทำได้ดีแบบนี้ต่อไปไหม", personality: "perfectionist" }
        ]
      },
      {
        id: 8,
        text: "ในกลุ่มเพื่อน คุณมักเป็น?",
        options: [
          { id: "a", text: "คนฟัง ให้คำปรึกษา", personality: "listener" },
          { id: "b", text: "คนนำ จัดการกิจกรรม", personality: "leader" },
          { id: "c", text: "คนสร้างความสนุก บรรยากาศดี", personality: "entertainer" },
          { id: "d", text: "คนช่วยเหลือ สนับสนุน", personality: "supporter" }
        ]
      },
      {
        id: 9,
        text: "เมื่อต้องทำงานเป็นทีม คุณชอบ?",
        options: [
          { id: "a", text: "เป็นผู้นำ ควบคุมทิศทาง", personality: "director" },
          { id: "b", text: "แบ่งงานทำตามความถนัด", personality: "coordinator" },
          { id: "c", text: "สนับสนุนและช่วยเหลือทุกคน", personality: "teamplayer" },
          { id: "d", text: "ทำงานอิสระในส่วนที่ได้รับมอบหมาย", personality: "independent" }
        ]
      },
      {
        id: 10,
        text: "เป้าหมายในอนาคตของคุณคือ?",
        options: [
          { id: "a", text: "มีครอบครัวที่อบอุ่น มีความสุข", personality: "family" },
          { id: "b", text: "ประสบความสำเร็จในอาชีพการงาน", personality: "success" },
          { id: "c", text: "เดินทาง สัมผัสประสบการณ์ใหม่ๆ", personality: "adventurer" },
          { id: "d", text: "มีอิสระทางการเงิน ไม่ต้องกังวล", personality: "financial" }
        ]
      }
    ];

    function calculatePersonality() {
      const counts = {};
      Object.values(answers).forEach(personality => {
        counts[personality] = (counts[personality] || 0) + 1;
      });

      const sortedPersonalities = Object.entries(counts).sort((a, b) => b[1] - a[1]);
      const topPersonality = sortedPersonalities[0][0];

      const personalities = {
        introvert: { title: "ผู้ใคร่ครวญ", desc: "คุณชอบความสงบ เวลาส่วนตัว และการไตร่ตรองอย่างลึกซึ้ง" },
        extrovert: { title: "ผู้สร้างพลัง", desc: "คุณได้พลังจากการพบปะผู้คน และชอบสร้างความสัมพันธ์" },
        active: { title: "ผู้กระตือรือร้น", desc: "คุณเต็มไปด้วยพลังงาน ชอบท้าทายและกิจกรรมต่างๆ" },
        learner: { title: "ผู้แสวงหาความรู้", desc: "คุณหิวกระหายความรู้ และชอบเรียนรู้สิ่งใหม่อยู่เสมอ" },
        analytical: { title: "นักวิเคราะห์", desc: "คุณคิดอย่างรอบคอบ วิเคราะห์ปัญหาอย่างเป็นระบบ" },
        collaborative: { title: "ผู้ร่วมงาน", desc: "คุณเชื่อในพลังของการทำงานเป็นทีม และชอบความร่วมมือ" },
        action: { title: "ผู้ปฏิบัติ", desc: "คุณเป็นคนลงมือทำ ไม่ชอบรอคอย และปรับตัวเก่ง" },
        researcher: { title: "นักค้นคว้า", desc: "คุณชอบหาข้อมูล วิจัย และทำความเข้าใจอย่างถี่ถ้วน" },
        emotional: { title: "ผู้รับรู้อารมณ์", desc: "คุณเชื่อในสัญชาตญาณ และให้ความสำคัญกับความรู้สึก" },
        logical: { title: "นักคิดเชิงตรรกะ", desc: "คุณใช้เหตุผล ข้อมูล และความคิดเป็นระบบในการตัดสินใจ" },
        experienced: { title: "ผู้มีประสบการณ์", desc: "คุณเรียนรู้จากอดีต และใช้บทเรียนชีวิตในการตัดสินใจ" },
        social: { title: "ผู้เข้าสังคม", desc: "คุณให้ความสำคัญกับความคิดเห็นของคนรอบข้าง" },
        relationship: { title: "ผู้ให้คุณค่าความสัมพันธ์", desc: "คนที่คุณรัก และความสัมพันธ์คือหัวใจของคุณ" },
        career: { title: "ผู้มุ่งมั่นในงาน", desc: "ความสำเร็จในหน้าที่การงานคือแรงขับเคลื่อนชีวิตคุณ" },
        growth: { title: "ผู้พัฒนาตนเอง", desc: "การเติบโตและพัฒนาศักยภาพคือเป้าหมายสำคัญของคุณ" },
        balance: { title: "ผู้แสวงหาสมดุล", desc: "คุณให้ความสำคัญกับความสุขและความสมดุลในทุกมิติของชีวิต" },
        creative: { title: "ผู้สร้างสรรค์", desc: "จินตนาการและความคิดสร้างสรรค์คือจุดแข็งของคุณ" },
        organized: { title: "ผู้จัดระเบียบ", desc: "ความเป็นระเบียบและการวางแผนคือวิถีชีวิตของคุณ" },
        flexible: { title: "ผู้ยืดหยุ่น", desc: "คุณปรับตัวได้ดี และรับมือกับการเปลี่ยนแปลงได้อย่างง่ายดาย" },
        confident: { title: "ผู้มั่นใจ", desc: "ความมั่นใจในตัวเองและความเชื่อมั่นคือจุดแข็งของคุณ" },
        artistic: { title: "ศิลปิน", desc: "คุณมีความสามารถทางศิลปะ และชอบแสดงออกอย่างสร้างสรรค์" },
        athletic: { title: "นักกีฬา", desc: "คุณรักการเคลื่อนไหว กีฬา และการดูแลสุขภาพ" },
        technical: { title: "นักเทคนิค", desc: "คุณสนใจเทคโนโลยี วิทยาศาสตร์ และสิ่งที่ท้าทายความคิด" },
        motivated: { title: "ผู้รับแรงบันดาลใจ", desc: "คำชมและกำลังใจคือเชื้อเพลิงที่ขับเคลื่อนคุณ" },
        humble: { title: "ผู้ถอมตน", desc: "คุณไม่ชอบโอ้อวด และมีความถ่อมตนในความสำเร็จ" },
        proud: { title: "ผู้ภาคภูมิใจ", desc: "คุณภูมิใจในความสำเร็จและใช้มันเป็นแรงผลักดัน" },
        perfectionist: { title: "ผู้ใฝ่ความสมบูรณ์แบบ", desc: "คุณตั้งมาตรฐานสูง และมุ่งมั่นทำทุกอย่างให้ดีที่สุด" },
        listener: { title: "ผู้รับฟัง", desc: "คุณเป็นคนรับฟังที่ดี และให้คำปรึกษาที่มีคุณค่า" },
        leader: { title: "ผู้นำ", desc: "คุณมีความสามารถในการนำ จัดการ และสร้างแรงบันดาลใจ" },
        entertainer: { title: "ผู้สร้างความสนุก", desc: "คุณสร้างบรรยากาศดี และทำให้คนรอบข้างมีความสุข" },
        supporter: { title: "ผู้สนับสนุน", desc: "คุณชอบช่วยเหลือและสนับสนุนคนอื่นให้ประสบความสำเร็จ" },
        director: { title: "ผู้กำกับ", desc: "คุณชอบควบคุมทิศทางและนำทีมไปสู่เป้าหมาย" },
        coordinator: { title: "ผู้ประสานงาน", desc: "คุณเชี่ยวชาญในการจัดการและประสานความร่วมมือ" },
        teamplayer: { title: "ผู้เล่นเป็นทีม", desc: "คุณเป็นสมาชิกทีมที่ยอดเยี่ยม สนับสนุนทุกคน" },
        independent: { title: "ผู้อิสระ", desc: "คุณชอบทำงานอิสระ และเก่งในการจัดการตัวเอง" },
        family: { title: "ผู้รักครอบครัว", desc: "ครอบครัวและความอบอุ่นคือความสุขที่แท้จริงของคุณ" },
        success: { title: "ผู้แสวงหาความสำเร็จ", desc: "ความสำเร็จในอาชีพคือเป้าหมายสำคัญของคุณ" },
        adventurer: { title: "นักผจญภัย", desc: "คุณรักการเดินทาง ผจญภัย และประสบการณ์ใหม่ๆ" },
        financial: { title: "ผู้แสวงหาอิสรภาพทางการเงิน", desc: "ความมั่นคงทางการเงินคือรากฐานสำคัญของชีวิตคุณ" }
      };

      return personalities[topPersonality] || { title: "ผู้มีเอกลักษณ์", desc: "คุณมีบุคลิกที่เป็นเอกลักษณ์และไม่เหมือนใคร" };
    }

    async function onConfigChange(config) {
      const appDiv = document.getElementById('app');
      const bgColor = config.background_color || defaultConfig.background_color;
      const cardColor = config.card_color || defaultConfig.card_color;
      const textColor = config.text_color || defaultConfig.text_color;
      const primaryColor = config.primary_button_color || defaultConfig.primary_button_color;
      const secondaryColor = config.secondary_button_color || defaultConfig.secondary_button_color;

      document.body.style.background = bgColor;

      if (!showingResult) {
        const question = questions[currentQuestion];
        const progress = ((currentQuestion) / questions.length) * 100;

        appDiv.innerHTML = `
          <div class="w-full min-h-full flex items-center justify-center p-6">
            <div class="w-full max-w-2xl">
              <div class="rounded-2xl shadow-2xl p-8" style="background-color: ${cardColor};">
                <div class="mb-8 text-center">
                  <h1 class="text-4xl font-bold mb-3" style="color: ${textColor};">${config.quiz_title || defaultConfig.quiz_title}</h1>
                  <p class="text-lg opacity-75" style="color: ${textColor};">${config.quiz_description || defaultConfig.quiz_description}</p>
                </div>

                <div class="mb-8">
                  <div class="flex justify-between items-center mb-2">
                    <span class="text-sm font-semibold" style="color: ${textColor};">ความคืบหน้า</span>
                    <span class="text-sm font-semibold" style="color: ${textColor};">${currentQuestion}/${questions.length}</span>
                  </div>
                  <div class="w-full h-3 rounded-full" style="background-color: ${secondaryColor};">
                    <div class="h-3 rounded-full transition-all duration-300" style="width: ${progress}%; background-color: ${primaryColor};"></div>
                  </div>
                </div>

                <div class="mb-8">
                  <h2 class="text-2xl font-semibold mb-6" style="color: ${textColor};">
                    ${question.id}. ${question.text}
                  </h2>
                  <div class="space-y-3">
                    ${question.options.map(option => `
                      <button 
                        class="option-btn w-full text-left p-4 rounded-xl font-medium transition-all duration-200 hover:scale-105 hover:shadow-lg"
                        style="background-color: ${secondaryColor}; color: ${textColor};"
                        data-personality="${option.personality}"
                      >
                        ${option.text}
                      </button>
                    `).join('')}
                  </div>
                </div>

                ${currentQuestion > 0 ? `
                  <button 
                    id="back-btn"
                    class="px-6 py-3 rounded-lg font-semibold transition-all duration-200 hover:scale-105"
                    style="background-color: ${secondaryColor}; color: ${textColor};"
                  >
                    ← ย้อนกลับ
                  </button>
                ` : ''}
              </div>
            </div>
          </div>
        `;

        document.querySelectorAll('.option-btn').forEach(btn => {
          btn.addEventListener('click', () => {
            const personality = btn.getAttribute('data-personality');
            answers[currentQuestion] = personality;
            
            if (currentQuestion < questions.length - 1) {
              currentQuestion++;
              onConfigChange(window.elementSdk.config);
            } else {
              showingResult = true;
              onConfigChange(window.elementSdk.config);
            }
          });
        });

        const backBtn = document.getElementById('back-btn');
        if (backBtn) {
          backBtn.addEventListener('click', () => {
            currentQuestion--;
            onConfigChange(window.elementSdk.config);
          });
        }
      } else {
        const result = calculatePersonality();

        appDiv.innerHTML = `
          <div class="w-full min-h-full flex items-center justify-center p-6">
            <div class="w-full max-w-2xl">
              <div class="rounded-2xl shadow-2xl p-8 text-center" style="background-color: ${cardColor};">
                <div class="mb-8">
                  <div class="text-6xl mb-4">🎉</div>
                  <h1 class="text-4xl font-bold mb-3" style="color: ${textColor};">${config.result_title || defaultConfig.result_title}</h1>
                </div>

                <div class="mb-8 p-6 rounded-xl" style="background-color: ${primaryColor};">
                  <h2 class="text-3xl font-bold mb-3 text-white">
                    ${result.title}
                  </h2>
                  <p class="text-xl text-white opacity-90">
                    ${result.desc}
                  </p>
                </div>

                <div class="mb-8 text-left p-6 rounded-xl" style="background-color: ${bgColor};">
                  <h3 class="text-xl font-semibold mb-4" style="color: ${textColor};">💡 สรุปคำตอบของคุณ</h3>
                  <div class="space-y-2">
                    ${Object.entries(answers).map(([questionNum, personality]) => `
                      <div class="text-sm" style="color: ${textColor};">
                        <span class="font-semibold">ข้อ ${parseInt(questionNum) + 1}:</span> ${questions[questionNum].options.find(o => o.personality === personality).text}
                      </div>
                    `).join('')}
                  </div>
                </div>

                <button 
                  id="restart-btn"
                  class="px-8 py-4 rounded-xl font-bold text-lg transition-all duration-200 hover:scale-105 hover:shadow-lg"
                  style="background-color: ${primaryColor}; color: white;"
                >
                  ทำแบบทดสอบอีกครั้ง
                </button>
              </div>
            </div>
          </div>
        `;

        document.getElementById('restart-btn').addEventListener('click', () => {
          currentQuestion = 0;
          answers = {};
          showingResult = false;
          onConfigChange(window.elementSdk.config);
        });
      }
    }

    window.elementSdk.init({
      defaultConfig,
      onConfigChange,
      mapToCapabilities: (config) => ({
        recolorables: [
          {
            get: () => config.background_color || defaultConfig.background_color,
            set: (value) => {
              config.background_color = value;
              window.elementSdk.setConfig({ background_color: value });
            }
          },
          {
            get: () => config.card_color || defaultConfig.card_color,
            set: (value) => {
              config.card_color = value;
              window.elementSdk.setConfig({ card_color: value });
            }
          },
          {
            get: () => config.text_color || defaultConfig.text_color,
            set: (value) => {
              config.text_color = value;
              window.elementSdk.setConfig({ text_color: value });
            }
          },
          {
            get: () => config.primary_button_color || defaultConfig.primary_button_color,
            set: (value) => {
              config.primary_button_color = value;
              window.elementSdk.setConfig({ primary_button_color: value });
            }
          },
          {
            get: () => config.secondary_button_color || defaultConfig.secondary_button_color,
            set: (value) => {
              config.secondary_button_color = value;
              window.elementSdk.setConfig({ secondary_button_color: value });
            }
          }
        ],
        borderables: [],
        fontEditable: undefined,
        fontSizeable: undefined
      }),
      mapToEditPanelValues: (config) => new Map([
        ["quiz_title", config.quiz_title || defaultConfig.quiz_title],
        ["quiz_description", config.quiz_description || defaultConfig.quiz_description],
        ["result_title", config.result_title || defaultConfig.result_title]
      ])
    });
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a419cb7e2b33e4f',t:'MTc2NDA3ODU5Ni4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
