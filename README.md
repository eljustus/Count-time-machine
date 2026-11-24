# Count-time-machine
Created from prompts on Gemini
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Oceanic Glass Countdown</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;600;700&display=swap');

    :root {
      --bg-gradient-start: #072b29;
      --bg-gradient-end: #0d4f4b;
      --glass-surface: rgba(224, 247, 250, 0.10);
      --glass-border: rgba(224, 247, 250, 0.22);
      --accent-seafoam: #2ee59d;
      --text-primary: #f0fdfa;
      --text-muted: #99f6e4;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, var(--bg-gradient-start), var(--bg-gradient-end));
      font-family: 'Space Grotesk', sans-serif;
      color: var(--text-primary);
      padding: 1.5rem;
    }

    .countdown-wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 1.5rem;
    }

    .status-badge {
      font-size: 0.75rem;
      font-weight: 600;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: var(--accent-seafoam);
      background: var(--glass-surface);
      border: 1px solid var(--glass-border);
      padding: 0.4rem 0.9rem;
      border-radius: 0;
    }

    .timer-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1rem;
    }

    .time-block {
      width: 115px;
      height: 115px;
      background: var(--glass-surface);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid var(--glass-border);
      border-radius: 0; /* Strict square profile */
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: relative;
      transition: border-color 0.2s ease, transform 0.2s ease;
    }

    .time-block::after {
      content: "";
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 3px;
      background: var(--accent-seafoam);
      opacity: 0.7;
    }

    .time-block:hover {
      border-color: var(--accent-seafoam);
      transform: translateY(-2px);
    }

    .digit {
      font-size: 2.75rem;
      font-weight: 700;
      line-height: 1;
      font-variant-numeric: tabular-nums; /* Prevents jitter during ticks */
      color: var(--text-primary);
    }

    .label {
      margin-top: 0.5rem;
      font-size: 0.7rem;
      font-weight: 600;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      color: var(--text-muted);
    }

    .expired .time-block {
      border-color: var(--accent-seafoam);
    }

    @media (max-width: 540px) {
      .timer-grid {
        grid-template-columns: repeat(2, 1fr);
      }
      .time-block {
        width: 105px;
        height: 105px;
      }
    }
  </style>
</head>
<body>

  <div class="countdown-wrapper" id="countdown-container">
    <div class="status-badge" id="status-text">Target: 09:00 UTC</div>
    
    <div class="timer-grid">
      <div class="time-block">
        <span class="digit" id="days">00</span>
        <span class="label">Days</span>
      </div>
      <div class="time-block">
        <span class="digit" id="hours">00</span>
        <span class="label">Hours</span>
      </div>
      <div class="time-block">
        <span class="digit" id="minutes">00</span>
        <span class="label">Minutes</span>
      </div>
      <div class="time-block">
        <span class="digit" id="seconds">00</span>
        <span class="label">Seconds</span>
      </div>
    </div>
  </div>

  <script>
    // Locked to November 24, 2026 at 09:00:00 UTC
    const TARGET_UTC_TIMESTAMP = Date.UTC(2026, 10, 24, 9, 0, 0);

    const els = {
      container: document.getElementById('countdown-container'),
      status: document.getElementById('status-text'),
      days: document.getElementById('days'),
      hours: document.getElementById('hours'),
      minutes: document.getElementById('minutes'),
      seconds: document.getElementById('seconds')
    };

    const pad = (num) => String(num).padStart(2, '0');

    function updateTimer() {
      const diff = TARGET_UTC_TIMESTAMP - Date.now();

      if (diff <= 0) {
        els.days.textContent = '00';
        els.hours.textContent = '00';
        els.minutes.textContent = '00';
        els.seconds.textContent = '00';
        els.status.textContent = 'Deadline Reached';
        els.container.classList.add('expired');
        clearInterval(intervalId);
        return;
      }

      const days = Math.floor(diff / (1000 * 60 * 60 * 24));
      const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const minutes = Math.floor((diff / (1000 * 60)) % 60);
      const seconds = Math.floor((diff / 1000) % 60);

      els.days.textContent = pad(days);
      els.hours.textContent = pad(hours);
      els.minutes.textContent = pad(minutes);
      els.seconds.textContent = pad(seconds);
    }

    updateTimer();
    const intervalId = setInterval(updateTimer, 1000);
  </script>
</body>
</html>
