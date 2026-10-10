<!DOCTYPE html>  
<html lang="ru">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">  
    <meta name="apple-mobile-web-app-capable" content="yes">  
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">  
    <meta name="apple-mobile-web-app-title" content="Deutsch A1">  
    <meta name="theme-color" content="#000000">  
    <title>Deutsch Lernen - A1.1</title>  
    <link rel="manifest" href="manifest.json">  
    <link rel="apple-touch-icon" href="icons/icon-192.png">  
    <style>  
        * { margin: 0; padding: 0; box-sizing: border-box; user-select: none; -webkit-tap-highlight-color: transparent; }  
        body { background: #000; color: white; font-family: 'Arial Black', Arial, sans-serif; min-height: 100vh; }  
          
        /* Экран загрузки */  
        #loading-screen { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: #000; display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; gap: 28px; }  
        .flag-bar { width: 100%; height: 8px; background: linear-gradient(90deg, #000 0%, #DD0000 50%, #FFCE00 100%); position: fixed; top: 0; }  
        .loading-title { font-size: 2.4rem; letter-spacing: 0.1em; text-transform: uppercase; border: 4px solid white; padding: 20px 40px; text-align: center; line-height: 1.3; }  
        .loading-subtitle { color: #aaa; font-size: 0.9rem; letter-spacing: 0.2em; text-transform: uppercase; }  
        .loading-bar-wrap { width: 280px; height: 8px; background: #111; border: 2px solid #333; }  
        .loading-bar { height: 100%; width: 0%; background: linear-gradient(90deg, #DD0000, #FFCE00); transition: width 0.3s ease; }  
        #loading-status { color: #555; font-size: 0.72rem; letter-spacing: 0.15em; font-family: monospace; }  
          
        /* Основное приложение */  
        #app { display: none; max-width: 600px; margin: 0 auto; padding: 20px; min-height: 100vh; background: #000; }  
        .top-bar { height: 8px; background: linear-gradient(90deg, #000, #DD0000, #FFCE00); margin-bottom: 20px; }  
        .question-header { display: flex; justify-content: space-between; margin-bottom: 20px; font-size: 14px; color: #aaa; }  
        .progress-bar { width: 100%; height: 8px; background: #111; border: 2px solid #333; margin-bottom: 30px; }  
        .progress-fill { height: 100%; width: 0%; background: linear-gradient(90deg, #DD0000, #FFCE00); transition: width 0.3s ease; }  
        h1 { text-align: center; border: 4px solid white; padding: 18px; margin-bottom: 20px; font-size: 1.8rem; }  
        h2 { text-align: center; font-size: 2.2rem; margin: 30px 0; word-break: break-word; }  
        .german-word { text-align: center; font-size: 2.2rem; margin: 30px 0; font-weight: bold; letter-spacing: 0.05em; }  
        input { width: 100%; background: #111; color: white; border: 3px solid white; font-size: 17px; padding: 12px; border-radius: 0; margin: 15px 0; font-family: Arial, sans-serif; }  
        input::placeholder { color: #555; }  
        button { width: 100%; background: white; color: black; border: 3px solid black; font-weight: bold; font-size: 16px; padding: 14px 20px; text-transform: uppercase; letter-spacing: 0.1em; cursor: pointer; transition: all 0.15s; font-family: 'Arial Black', Arial, sans-serif; margin: 8px 0; }  
        button:active { transform: translate(3px, 3px); box-shadow: none; }  
        .success-box { background: #004d00; border: 4px solid #00cc00; padding: 14px; text-align: center; margin: 15px 0; font-weight: bold; }  
        .error-box { background: #4d0000; border: 4px solid #DD0000; padding: 14px; text-align: center; margin: 15px 0; font-weight: bold; }  
        .result-box { background: black; border: 4px solid white; padding: 25px; text-align: center; margin: 20px 0; }  
        .rank-emoji { font-size: 72px; text-align: center; margin: 10px 0; }  
        .rank-title { text-align: center; font-size: 2rem; margin: 10px 0; }  
        .score-large { font-size: 3.2rem; margin: 12px 0; }  
        .two-columns { display: flex; gap: 15px; margin-top: 20px; }  
        .two-columns button { margin: 0; }  
        .info-box { background: #4d3300; border: 4px solid #FFCE00; padding: 14px; text-align: center; margin: 15px 0; font-weight: bold; }  
        @media (max-width: 600px) { #app { padding: 15px; } .german-word { font-size: 1.8rem; } }  
    </style>  
</head>  
<body>  
  
<div id="loading-screen">  
    <div class="flag-bar"></div>  
    <div class="loading-title">DEUTSCH<br>LERNEN</div>  
    <div class="loading-subtitle">Модуль A1.1 · Тема 1</div>  
    <div class="loading-bar-wrap"><div class="loading-bar" id="loading-bar"></div></div>  
    <div id="loading-status">ЗАГРУЗКА...</div>  
</div>  
  
<div id="app">  
    <div class="top-bar"></div>  
    <div id="main-content"></div>  
</div>  
  
<script>  
    // Вопросы  
    const questions = [  
        {german: "Hallo", russian: "привет"},  
        {german: "Guten Morgen", russian: "доброе утро"},  
        {german: "Guten Tag", russian: "добрый день"},  
        {german: "Guten Abend", russian: "добрый вечер"},  
        {german: "Gute Nacht", russian: "спокойной ночи"},  
        {german: "Auf Wiedersehen", russian: "до свидания"},  
        {german: "Tschüs", russian: "пока"},  
        {german: "Ich heiße", russian: "меня зовут"},  
        {german: "Mein Name ist", russian: "моё имя", alternatives: ["мое имя"]},  
        {german: "Das ist", russian: "это", alternatives: ["это есть"]},  
        {german: "Wer", russian: "кто"},  
        {german: "Wie", russian: "как"},  
        {german: "die Frau", russian: "женщина"},  
        {german: "der Herr", russian: "мужчина"},  
        {german: "der Name", russian: "имя"},  
        {german: "der Vorname", russian: "имя"},  
        {german: "der Familienname", russian: "фамилия"},  
        {german: "Woher", russian: "откуда"},  
        {german: "Kommen aus", russian: "родом из"},  
        {german: "Aus", russian: "из"},  
        {german: "das Land", russian: "страна"},  
        {german: "Deutschland", russian: "германия"},  
        {german: "Länder", russian: "страны"},  
        {german: "Österreich", russian: "австрия"},  
        {german: "die Schweiz", russian: "швейцария"},  
        {german: "Wie geht's", russian: "как дела"},  
        {german: "sehr gut", russian: "очень хорошо"},  
        {german: "gut", russian: "хорошо"},  
        {german: "es geht", russian: "нормально"},  
        {german: "nicht so gut", russian: "не так хорошо"},  
        {german: "das Alphabet", russian: "алфавит"},  
        {german: "Buchstabieren", russian: "произносить по буквам"},  
        {german: "auch", russian: "также", alternatives: ["так же"]},  
        {german: "Danke", russian: "спасибо"},  
        {german: "Ja", russian: "да"},  
        {german: "nein", russian: "нет"},  
        {german: "sehr", russian: "очень"},  
        {german: "und", russian: "и"}  
    ];  
      
    // Состояние  
    let screen = 'start';  
    let currentQuestion = 0;  
    let score = 0;  
    let showResult = false;  
    let lastCorrect = false;  
    let userAnswer = '';  
      
    // DOM элементы  
    const loadingScreen = document.getElementById('loading-screen');  
    const appDiv = document.getElementById('app');  
    const mainContent = document.getElementById('main-content');  
    const loadingBar = document.getElementById('loading-bar');  
    const loadingStatus = document.getElementById('loading-status');  
      
    // Имитация загрузки  
    let progress = 0;  
    const loadInterval = setInterval(() => {  
        progress += 10;  
        loadingBar.style.width = progress + '%';  
        if (progress >= 100) {  
            clearInterval(loadInterval);  
            loadingStatus.textContent = 'ГОТОВО!';  
            setTimeout(() => {  
                loadingScreen.style.display = 'none';  
                appDiv.style.display = 'block';  
                render();  
            }, 500);  
        } else if (progress === 30) loadingStatus.textContent = 'ЗАГРУЗКА МОДУЛЕЙ...';  
        else if (progress === 60) loadingStatus.textContent = 'ПОДГОТОВКА СЛОВАРЯ...';  
        else if (progress === 80) loadingStatus.textContent = 'ЗАПУСК...';  
    }, 150);  
      
    function checkAnswer(userAnswer, question) {  
        const normalized = userAnswer.trim().toLowerCase();  
        if (normalized === question.russian.toLowerCase()) return true;  
        if (question.alternatives) {  
            for (let alt of question.alternatives) {  
                if (normalized === alt.toLowerCase()) return true;  
            }  
        }  
        return false;  
    }  
      
    function getRank(score, total) {  
        if (score >= 35) return {title: "МОЛОДЕЦ!", color: "#00ff88", emoji: "🎉"};  
        if (score >= 28) return {title: "ОТЛИЧНО!", color: "#00d4ff", emoji: "🔥"};  
        if (score >= 20) return {title: "ХОРОШО!", color: "#ffa500", emoji: "👍"};  
        if (score >= 15) return {title: "НУЖНО ПОВТОРИТЬ", color: "#ffa500", emoji: "📚"};  
        if (score >= 10) return {title: "ПРОЙДИ ЕЩЁ РАЗ", color: "#ff6b6b", emoji: "😊"};  
        return {title: "НАЧНИ С НАЧАЛА", color: "#ff3366", emoji: "📖"};  
    }  
      
    function render() {  
        if (screen === 'start') {  
            mainContent.innerHTML = `  
                <h1>DEUTSCH LERNEN</h1>  
                <div style="background:black;border:2px solid #333;border-left:6px solid #DD0000;padding:18px;margin:15px 0;">  
                    <h3 style="margin:0 0 8px 0;">МОДУЛЬ A1.1 · ТЕМА 1</h3>  
                    <p style="color:#ccc;margin:0;">Базовые приветствия и знакомство<br><strong style="color:#FFCE00;">${questions.length} слов</strong></p>  
                </div>  
                <div style="background:#0a0a0a;border:2px solid #222;padding:14px;margin:12px 0;">  
                    <p style="color:#777;font-size:0.75em;text-transform:uppercase;">Как это работает</p>  
                    <p style="color:#bbb;margin:5px 0;">📖 Видишь немецкое слово</p>  
                    <p style="color:#bbb;margin:5px 0;">✏️ Вводишь перевод на русском</p>  
                    <p style="color:#bbb;margin:5px 0;">🏆 Получаешь звание по итогу</p>  
                </div>  
                <button onclick="startQuiz()">НАЧАТЬ ТЕСТ</button>  
            `;  
        }   
        else if (screen === 'quiz') {  
            const q = questions[currentQuestion];  
            const progressPercent = ((currentQuestion + 1) / questions.length) * 100;  
              
            let resultHtml = '';  
            if (showResult) {  
                if (lastCorrect) {  
                    resultHtml = `<div class="success-box">✅ ПРАВИЛЬНО!<br><span style="font-size:0.85em;">${q.german} — ${q.russian}</span></div>`;  
                } else {  
                    resultHtml = `<div class="error-box">❌ НЕПРАВИЛЬНО!<br><span style="font-size:0.85em;">${q.german} — ${q.russian}</span></div>`;  
                }  
            }  
              
            mainContent.innerHTML = `  
                <div class="question-header">  
                    <span>ВОПРОС ${currentQuestion + 1}/${questions.length}</span>  
                    <span>🏆 ${score}</span>  
                </div>  
                <div class="progress-bar"><div class="progress-fill" style="width: ${progressPercent}%;"></div></div>  
                <div class="german-word">${q.german}</div>  
                <input type="text" id="answerInput" placeholder="Введите перевод на русском..." ${showResult ? 'disabled' : ''}>  
                ${!showResult ? '<button onclick="checkAndShow()">ПРОВЕРИТЬ</button>' : resultHtml}  
                ${showResult ? (currentQuestion + 1 < questions.length ?   
                    '<button onclick="nextQuestion()">СЛЕДУЮЩИЙ ВОПРОС →</button>' :   
                    '<button onclick="finishQuiz()">ЗАВЕРШИТЬ ТЕСТ</button>') : ''}  
            `;  
            if (!showResult && document.getElementById('answerInput')) {  
                document.getElementById('answerInput').focus();  
            }  
        }   
        else if (screen === 'results') {  
            const rank = getRank(score, questions.length);  
            const pct = Math.round((score / questions.length) * 100);  
            mainContent.innerHTML = `  
                <div class="rank-emoji">${rank.emoji}</div>  
                <div class="rank-title" style="color: ${rank.color};">${rank.title}</div>  
                <div class="result-box">  
                    <p style="color:#888;font-size:0.8em;text-transform:uppercase;">Результат:</p>  
                    <p class="score-large">${score} / ${questions.length}</p>  
                    <p style="color:#FFCE00;">${pct}% правильных</p>  
                </div>  
                <div class="progress-bar"><div class="progress-fill" style="width: ${(score/questions.length)*100}%;"></div></div>  
                <div class="two-columns">  
                    <button onclick="restartQuiz()">🔄 ЗАНОВО</button>  
                    <button onclick="goHome()">🏠 НА ГЛАВНУЮ</button>  
                </div>  
            `;  
        }  
    }  
      
    function startQuiz() {  
        screen = 'quiz';  
        currentQuestion = 0;  
        score = 0;  
        showResult = false;  
        render();  
    }  
      
    function checkAndShow() {  
        const input = document.getElementById('answerInput');  
        if (!input.value.trim()) return;  
        const q = questions[currentQuestion];  
        lastCorrect = checkAnswer(input.value, q);  
        if (lastCorrect) score++;  
        showResult = true;  
        render();  
    }  
      
    function nextQuestion() {  
        currentQuestion++;  
        showResult = false;  
        render();  
    }  
      
    function finishQuiz() {  
        screen = 'results';  
        render();  
    }  
      
    function restartQuiz() {  
        screen = 'quiz';  
        currentQuestion = 0;  
        score = 0;  
        showResult = false;  
        render();  
    }  
      
    function goHome() {  
        screen = 'start';  
        render();  
    }  
</script>  
  
<!-- Service Worker для офлайн-режима -->  
<script>  
    if ('serviceWorker' in navigator) {  
        navigator.serviceWorker.register('service-worker.js').catch(e => console.log('SW failed:', e));  
    }  
</script>  
</body>  
</html>  
