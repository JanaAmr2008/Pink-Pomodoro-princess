<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pink Pomodoro Princess</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    /* Light Theme (Default) */
    :root {
      --primary: #ffb6c1; /* Light pink */
      --secondary: #ffdfe4; /* Lighter pink */
      --accent: #ff85a2; /* Medium pink */
      --text: #5a3a4a; /* Dark pinkish purple */
      --background: #fff9fb; /* Very light pink */
      --card: #ffffff; /* White */
      --shadow: rgba(255, 182, 193, 0.3);
      --timer-active: #ff85a2;
      --timer-rest: #8fd3f4; /* Soft blue for rest periods */
    }

    /* Dark Theme */
    [data-theme="dark"] {
      --primary: #d9578c; /* Darker pink */
      --secondary: #3a2a35; /* Dark purple */
      --accent: #ff85a2; /* Kept same for consistency */
      --text: #ffdfe4; /* Light pink */
      --background: #2a1e25; /* Very dark purple */
      --card: #3a2a35; /* Dark purple */
      --shadow: rgba(217, 87, 140, 0.3);
      --timer-active: #ff85a2;
      --timer-rest: #7ec8e3;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      transition: background-color 0.3s, color 0.3s;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--background);
      color: var(--text);
      min-height: 100vh;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
      border-bottom: 1px solid var(--primary);
      margin-bottom: 30px;
    }

    .logo {
      font-size: 28px;
      font-weight: 700;
      color: var(--accent);
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo i {
      font-size: 32px;
    }

    .theme-toggle {
      background: var(--primary);
      color: var(--text);
      border: none;
      padding: 8px 15px;
      border-radius: 20px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-weight: 600;
    }

    .main-content {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 30px;
    }

    @media (max-width: 768px) {
      .main-content {
        grid-template-columns: 1fr;
      }
    }

    .section {
      background-color: var(--card);
      border-radius: 20px;
      padding: 25px;
      box-shadow: 0 5px 15px var(--shadow);
    }

    .section-title {
      font-size: 22px;
      margin-bottom: 20px;
      color: var(--accent);
      display: flex;
      align-items: center;
      gap: 10px;
    }

    /* Todo List Styles */
    .todo-input {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    #item-input {
      flex: 1;
      padding: 12px 15px;
      border: 2px solid var(--secondary);
      border-radius: 12px;
      font-size: 16px;
      background-color: var(--card);
      color: var(--text);
    }

    #add-button {
      background-color: var(--accent);
      color: white;
      border: none;
      padding: 0 20px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 16px;
      font-weight: 600;
    }

    #add-button:hover {
      opacity: 0.9;
    }

    #item-list {
      list-style: none;
    }

    #item-list li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 15px;
      margin-bottom: 10px;
      background-color: var(--secondary);
      border-radius: 12px;
      animation: fadeIn 0.3s ease-out;
    }

    .remove-btn {
      background: none;
      border: none;
      color: var(--accent);
      cursor: pointer;
      font-size: 18px;
    }

    /* Pomodoro Timer Styles */
    .timer-container {
      text-align: center;
    }

    .timer-display {
      font-size: 72px;
      font-weight: 700;
      color: var(--timer-active);
      margin: 20px 0;
      font-family: 'Courier New', monospace;
    }

    .timer-controls {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin-bottom: 20px;
    }

    .timer-btn {
      padding: 10px 20px;
      border: none;
      border-radius: 12px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
    }

    .start-btn {
      background-color: var(--accent);
      color: white;
    }

    .reset-btn {
      background-color: var(--secondary);
      color: var(--text);
    }

    .pomodoro-settings {
      display: flex;
      justify-content: space-around;
      margin-top: 20px;
    }

    .setting {
      text-align: center;
    }

    .setting label {
      display: block;
      margin-bottom: 5px;
      font-weight: 600;
    }

    .setting input {
      width: 60px;
      padding: 5px;
      text-align: center;
      border: 2px solid var(--secondary);
      border-radius: 8px;
      background-color: var(--card);
      color: var(--text);
    }

    .timer-mode {
      font-size: 20px;
      margin-bottom: 10px;
      color: var(--accent);
    }

    /* Alarm notification */
    .alarm-notification {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: var(--accent);
      color: white;
      padding: 20px 30px;
      border-radius: 15px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.2);
      z-index: 100;
      display: none;
      animation: bounce 0.5s infinite alternate;
      text-align: center;
    }

    .alarm-notification i {
      font-size: 40px;
      margin-bottom: 10px;
      display: block;
    }

    .stop-alarm {
      background-color: white;
      color: var(--accent);
      border: none;
      padding: 8px 15px;
      border-radius: 10px;
      margin-top: 10px;
      font-weight: bold;
      cursor: pointer;
    }

    /* Animations */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.05); }
      100% { transform: scale(1); }
    }

    @keyframes bounce {
      from { transform: translate(-50%, -50%) scale(1); }
      to { transform: translate(-50%, -50%) scale(1.1); }
    }

    .pulse {
      animation: pulse 1s infinite;
    }
    @media (max-width: 768px) {
  .main-content { grid-template-columns: 1fr; }
}
@media (max-width: 400px) {
  .timer-display { font-size: 60px; }
}
  </style>
</head>
<body>
  <!-- Alarm Notification -->
  <div class="alarm-notification" id="alarm-notification">
    <i class="fas fa-bell"></i>
    <div id="alarm-message">Time's up!</div>
    <button class="stop-alarm" id="stop-alarm">Stop Alarm</button>
  </div>

  <div class="container">
    <header>
      <div class="logo">
        <i class="fas fa-crown"></i>
        <span>Pink Pomodoro Princess</span>
      </div>
      <button class="theme-toggle" id="theme-toggle">
        <i class="fas fa-moon"></i>
        <span>Dark Mode</span>
      </button>
    </header>

    <main class="main-content">
      <section class="section">
        <h2 class="section-title">
          <i class="fas fa-tasks"></i>
          My Tasks
        </h2>
        <div class="todo-input">
          <input type="text" id="item-input" placeholder="Add a new task...">
          <button id="add-button">Add</button>
        </div>
        <ul id="item-list"></ul>
      </section>

      <section class="section timer-container">
        <h2 class="section-title">
          <i class="fas fa-clock"></i>
          Pomodoro Timer
        </h2>
        <div class="timer-mode" id="timer-mode">Focus Time</div>
        <div class="timer-display" id="timer-display">25:00</div>
        <div class="timer-controls">
          <button class="timer-btn start-btn" id="start-btn">Start</button>
          <button class="timer-btn reset-btn" id="reset-btn">Reset</button>
        </div>
        <div class="pomodoro-settings">
          <div class="setting">
            <label for="focus-time">Focus (min)</label>
            <input type="number" id="focus-time" min="1" max="60" value="25">
          </div>
          <div class="setting">
            <label for="short-break">Short Break (min)</label>
            <input type="number" id="short-break" min="1" max="15" value="5">
          </div>
          <div class="setting">
            <label for="long-break">Long Break (min)</label>
            <input type="number" id="long-break" min="1" max="30" value="15">
          </div>
        </div>
      </section>
    </main>
  </div>

  <audio id="alarm-sound" loop>
    <source src="https://assets.mixkit.co/sfx/preview/mixkit-alarm-digital-clock-beep-989.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>
  <center>
    <footer>
  <p>
    &copy; 2025 Pink Pomodoro Princess<br>
    <small>A product of <strong>Taskify Solutions</strong> • Productivity with Pleasure</small>
  </p>
</footer>
  </center>
  <script>
    // Theme Toggle
    const themeToggle = document.getElementById('theme-toggle');
    const icon = themeToggle.querySelector('i');
    
    themeToggle.addEventListener('click', () => {
      document.body.dataset.theme = document.body.dataset.theme === 'dark' ? 'light' : 'dark';
      
      if (document.body.dataset.theme === 'dark') {
        icon.className = 'fas fa-sun';
        themeToggle.querySelector('span').textContent = 'Light Mode';
      } else {
        icon.className = 'fas fa-moon';
        themeToggle.querySelector('span').textContent = 'Dark Mode';
      }
    });

    // Todo List Functionality
    document.addEventListener('DOMContentLoaded', () => {
      const addButton = document.getElementById('add-button');
      const itemInput = document.getElementById('item-input');
      const itemList = document.getElementById('item-list');

      addButton.addEventListener('click', addTask);
      itemInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') addTask();
      });

      function addTask() {
        const text = itemInput.value.trim();
        if (!text) return;

        const li = document.createElement('li');
        li.innerHTML = `
          <span>${text}</span>
          <button class="remove-btn"><i class="fas fa-times"></i></button>
        `;

        li.querySelector('.remove-btn').addEventListener('click', () => {
          li.style.animation = 'fadeOut 0.3s ease-out';
          setTimeout(() => li.remove(), 300);
        });

        itemList.appendChild(li);
        itemInput.value = '';
      }
    });

    // Pomodoro Timer Functionality
    const timerDisplay = document.getElementById('timer-display');
    const startBtn = document.getElementById('start-btn');
    const resetBtn = document.getElementById('reset-btn');
    const timerMode = document.getElementById('timer-mode');
    const focusTimeInput = document.getElementById('focus-time');
    const shortBreakInput = document.getElementById('short-break');
    const longBreakInput = document.getElementById('long-break');
    const alarmNotification = document.getElementById('alarm-notification');
    const alarmMessage = document.getElementById('alarm-message');
    const stopAlarmBtn = document.getElementById('stop-alarm');
    const alarmSound = document.getElementById('alarm-sound');

    let timer;
    let timeLeft = 25 * 60; // 25 minutes in seconds
    let isRunning = false;
    let isFocusTime = true;
    let pomodoroCount = 0;

    function updateDisplay() {
      const minutes = Math.floor(timeLeft / 60);
      const seconds = timeLeft % 60;
      timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
      
      // Change color based on mode
      if (isFocusTime) {
        timerDisplay.style.color = 'var(--timer-active)';
        timerMode.textContent = 'Focus Time';
      } else {
        timerDisplay.style.color = 'var(--timer-rest)';
        timerMode.textContent = pomodoroCount % 4 === 0 ? 'Long Break' : 'Short Break';
      }
    }

    function showAlarm(message) {
      alarmMessage.textContent = message;
      alarmNotification.style.display = 'block';
      alarmSound.play();
    }

    function hideAlarm() {
      alarmNotification.style.display = 'none';
      alarmSound.pause();
      alarmSound.currentTime = 0;
    }

    function startTimer() {
      if (isRunning) {
        clearInterval(timer);
        startBtn.textContent = 'Start';
        isRunning = false;
        return;
      }

      isRunning = true;
      startBtn.textContent = 'Pause';
      
      timer = setInterval(() => {
        timeLeft--;
        updateDisplay();
        
        if (timeLeft <= 0) {
          clearInterval(timer);
          isRunning = false;
          timerDisplay.classList.add('pulse');
          
          // Show alarm notification
          if (isFocusTime) {
            showAlarm('Focus time complete! Take a break!');
          } else {
            showAlarm('Break time over! Ready to focus?');
          }
          
          // Switch mode
          if (isFocusTime) {
            pomodoroCount++;
            if (pomodoroCount % 4 === 0) {
              // Long break after 4 pomodoros
              timeLeft = parseInt(longBreakInput.value) * 60;
              showAlarm('Great job! Take a long break!');
            } else {
              // Short break
              timeLeft = parseInt(shortBreakInput.value) * 60;
            }
          } else {
            // Back to focus time
            timeLeft = parseInt(focusTimeInput.value) * 60;
          }
          
          isFocusTime = !isFocusTime;
          updateDisplay();
        }
      }, 1000);
    }

    function resetTimer() {
      clearInterval(timer);
      isRunning = false;
      startBtn.textContent = 'Start';
      isFocusTime = true;
      timeLeft = parseInt(focusTimeInput.value) * 60;
      updateDisplay();
      timerDisplay.classList.remove('pulse');
      hideAlarm();
    }

    startBtn.addEventListener('click', startTimer);
    resetBtn.addEventListener('click', resetTimer);
    stopAlarmBtn.addEventListener('click', () => {
      hideAlarm();
      startTimer(); // Auto-start next session
    });

    // Update timer when settings change
    [focusTimeInput, shortBreakInput, longBreakInput].forEach(input => {
      input.addEventListener('change', () => {
        if (!isRunning && isFocusTime) {
          timeLeft = parseInt(focusTimeInput.value) * 60;
          updateDisplay();
        }
      });
    });

    // Initialize
    updateDisplay();
  </script>
</body>
</html>
