[mosqawaycontroller.html](https://github.com/user-attachments/files/31345563/mosqawaycontroller.html)
# mosqaway<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MosqAway</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Manrope:wght@500;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap');

  :root {
    --bg: #0B1D26;
    --surface: #122C38;
    --surface-hi: #16374480;
    --line: #23414F;
    --accent: #C7D93E;
    --accent-dim: #C7D93E33;
    --danger: #E85C4A;
    --danger-dim: #E85C4A26;
    --text: #EAF2F0;
    --text-muted: #7C97A0;
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    min-height: 100vh;
    background:
      radial-gradient(ellipse 900px 500px at 50% -10%, #17414F 0%, transparent 60%),
      var(--bg);
    color: var(--text);
    font-family: 'Manrope', sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
  }

  .panel {
    width: 100%;
    max-width: 420px;
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 30px 80px -30px #00000080;
  }

  .header {
    padding: 22px 24px 18px;
    border-bottom: 1px solid var(--line);
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .brand {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .brand-name {
    font-weight: 800;
    font-size: 19px;
    letter-spacing: 0.3px;
  }

  .brand-sub {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10.5px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  .status-pill {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.5px;
    padding: 5px 10px 5px 8px;
    border-radius: 100px;
    border: 1px solid var(--line);
    color: var(--text-muted);
    transition: all .25s ease;
  }

  .status-dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--text-muted);
    transition: all .25s ease;
  }

  .status-pill.on {
    color: var(--accent);
    border-color: var(--accent-dim);
    background: var(--accent-dim);
  }
  .status-pill.on .status-dot {
    background: var(--accent);
    box-shadow: 0 0 0 4px var(--accent-dim);
  }

  /* connect stage */
  .connect-stage {
    padding: 40px 24px 32px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 22px;
  }

  .radar {
    position: relative;
    width: 120px;
    height: 120px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .radar-ring {
    position: absolute;
    inset: 0;
    border-radius: 50%;
    border: 1px solid var(--accent-dim);
    opacity: 0;
  }

  .radar.pulsing .radar-ring {
    animation: pulse 2.6s ease-out infinite;
  }
  .radar.pulsing .radar-ring:nth-child(2) { animation-delay: .9s; }
  .radar.pulsing .radar-ring:nth-child(3) { animation-delay: 1.8s; }

  @keyframes pulse {
    0%   { transform: scale(0.4); opacity: 0; }
    15%  { opacity: .9; }
    100% { transform: scale(1.15); opacity: 0; }
  }

  .radar-core {
    width: 54px;
    height: 54px;
    border-radius: 50%;
    background: var(--surface-hi);
    border: 1px solid var(--line);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 22px;
    z-index: 2;
    transition: all .3s ease;
  }
  .radar.pulsing .radar-core {
    border-color: var(--accent);
    box-shadow: 0 0 0 6px var(--accent-dim);
  }

  .connect-btn {
    width: 100%;
    padding: 14px;
    border-radius: 12px;
    border: none;
    background: var(--accent);
    color: #0B1D26;
    font-family: 'Manrope', sans-serif;
    font-weight: 800;
    font-size: 14.5px;
    letter-spacing: 0.2px;
    cursor: pointer;
    transition: transform .12s ease, opacity .2s ease;
  }
  .connect-btn:active { transform: scale(0.98); }
  .connect-btn:disabled { opacity: .5; cursor: default; }

  .connect-hint {
    font-size: 12.5px;
    color: var(--text-muted);
    text-align: center;
    line-height: 1.5;
    padding: 0 8px;
  }

  /* control stage */
  .controls {
    padding: 24px;
    display: none;
    flex-direction: column;
    gap: 18px;
  }
  .controls.show { display: flex; }
  .connect-stage.hide { display: none; }

  .section-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10.5px;
    letter-spacing: 1.8px;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  .power-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .btn {
    padding: 16px 10px;
    border-radius: 12px;
    border: 1px solid var(--line);
    background: var(--surface-hi);
    color: var(--text);
    font-family: 'Manrope', sans-serif;
    font-weight: 700;
    font-size: 14px;
    cursor: pointer;
    transition: all .15s ease;
  }
  .btn:active { transform: scale(0.97); }

  .btn-on:hover { border-color: var(--accent); color: var(--accent); }
  .btn-off:hover { border-color: var(--danger); color: var(--danger); }

  .btn-on.active {
    background: var(--accent-dim);
    border-color: var(--accent);
    color: var(--accent);
  }
  .btn-off.active {
    background: var(--danger-dim);
    border-color: var(--danger);
    color: var(--danger);
  }

  .timer-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .btn-timer {
    padding: 13px 10px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 500;
  }

  .countdown {
    display: none;
    align-items: center;
    justify-content: space-between;
    background: var(--accent-dim);
    border: 1px solid var(--accent);
    border-radius: 12px;
    padding: 12px 16px;
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent);
    font-size: 13px;
  }
  .countdown.show { display: flex; }

  .log {
    background: #0A1920;
    border: 1px solid var(--line);
    border-radius: 12px;
    padding: 12px 14px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11.5px;
    line-height: 1.75;
    color: var(--text-muted);
    max-height: 120px;
    overflow-y: auto;
  }
  .log div span { color: var(--accent); }
  .log div.err span { color: var(--danger); }

  .disconnect-link {
    text-align: center;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--text-muted);
    background: none;
    border: none;
    cursor: pointer;
    text-decoration: underline;
    text-underline-offset: 3px;
    padding: 4px;
  }

  @media (prefers-reduced-motion: reduce) {
    .radar.pulsing .radar-ring { animation: none; opacity: .3; }
  }
</style>
</head>
<body>

<div class="panel">
  <div class="header">
    <div class="brand">
      <div class="brand-name">MosqAway</div>
      <div class="brand-sub">Relay Control</div>
    </div>
    <div class="status-pill" id="statusPill">
      <div class="status-dot"></div>
      <span id="statusText">Offline</span>
    </div>
  </div>

  <div class="connect-stage" id="connectStage">
    <div class="radar" id="radar">
      <div class="radar-ring"></div>
      <div class="radar-ring"></div>
      <div class="radar-ring"></div>
      <div class="radar-core">🦟</div>
    </div>
    <button class="connect-btn" id="connectBtn">Connect Device</button>
    <div class="connect-hint">
      Requires Chrome or Edge (Android / desktop).<br>
      Not supported in Safari on iPhone.
    </div>
  </div>

  <div class="controls" id="controls">
    <div>
      <div class="section-label" style="margin-bottom:10px;">Power</div>
      <div class="power-row">
        <button class="btn btn-on" id="btnOn">ON</button>
        <button class="btn btn-off" id="btnOff">OFF</button>
      </div>
    </div>

    <div>
      <div class="section-label" style="margin-bottom:10px;">Timed session</div>
      <div class="timer-row">
        <button class="btn btn-timer" data-mins="30">30 MIN</button>
        <button class="btn btn-timer" data-mins="60">60 MIN</button>
      </div>
      <button class="btn btn-timer" data-secs="10" style="width:100%; margin-top:10px; opacity:.75;">10 SEC · test</button>
    </div>

    <div class="countdown" id="countdown">
      <span>Session active</span>
      <span id="countdownTime">--:--</span>
    </div>

    <div>
      <div class="section-label" style="margin-bottom:10px;">Log</div>
      <div class="log" id="log"></div>
    </div>

    <button class="disconnect-link" id="disconnectBtn">Disconnect</button>
  </div>
</div>

<script>
const SERVICE_UUID = "12345678-1234-1234-1234-123456789001";
const CHARACTERISTIC_UUID = "12345678-1234-1234-1234-123456789002";

let device, characteristic;
let countdownInterval = null;

const el = id => document.getElementById(id);
const logBox = el('log');

function logLine(text, isErr) {
  const line = document.createElement('div');
  if (isErr) line.classList.add('err');
  const time = new Date().toLocaleTimeString([], {hour12:false});
  line.innerHTML = `<span>${time}</span>  ${text}`;
  logBox.appendChild(line);
  logBox.scrollTop = logBox.scrollHeight;
}

function setConnectedUI(connected) {
  el('statusPill').classList.toggle('on', connected);
  el('statusText').textContent = connected ? 'Connected' : 'Offline';
  el('radar').classList.toggle('pulsing', !connected);
  el('connectStage').classList.toggle('hide', connected);
  el('controls').classList.toggle('show', connected);
}

async function connect() {
  try {
    el('connectBtn').disabled = true;
    el('connectBtn').textContent = 'Scanning…';
    logLine('Requesting device…');

    device = await navigator.bluetooth.requestDevice({
      filters: [{ name: 'MosqAway' }],
      optionalServices: [SERVICE_UUID]
    });

    device.addEventListener('gattserverdisconnected', onDisconnected);

    logLine(`Found: ${device.name}`);
    const server = await device.gatt.connect();
    const service = await server.getPrimaryService(SERVICE_UUID);
    characteristic = await service.getCharacteristic(CHARACTERISTIC_UUID);

    logLine('Connected to MosqAway');
    setConnectedUI(true);
  } catch (err) {
    logLine(`Error: ${err.message}`, true);
  } finally {
    el('connectBtn').disabled = false;
    el('connectBtn').textContent = 'Connect Device';
  }
}

function onDisconnected() {
  logLine('Device disconnected', true);
  setConnectedUI(false);
  clearCountdown();
}

async function sendCommand(cmd) {
  if (!characteristic) return;
  try {
    const encoder = new TextEncoder();
    await characteristic.writeValue(encoder.encode(cmd));
    logLine(`Sent: <span>${cmd}</span>`);
    highlightPower(cmd);
  } catch (err) {
    logLine(`Write failed: ${err.message}`, true);
  }
}

function highlightPower(cmd) {
  el('btnOn').classList.toggle('active', cmd === 'ON' || cmd === '30' || cmd === '60');
  el('btnOff').classList.toggle('active', cmd === 'OFF');
}

function startCountdown(mins) {
  clearCountdown();
  let secondsLeft = mins * 60;
  el('countdown').classList.add('show');
  updateCountdownDisplay(secondsLeft);

  countdownInterval = setInterval(() => {
    secondsLeft--;
    if (secondsLeft <= 0) {
      clearCountdown();
      highlightPower('OFF');
      return;
    }
    updateCountdownDisplay(secondsLeft);
  }, 1000);
}

function startCountdownSeconds(secs) {
  clearCountdown();
  let secondsLeft = secs;
  el('countdown').classList.add('show');
  updateCountdownDisplay(secondsLeft);

  countdownInterval = setInterval(() => {
    secondsLeft--;
    if (secondsLeft <= 0) {
      clearCountdown();
      highlightPower('OFF');
      return;
    }
    updateCountdownDisplay(secondsLeft);
  }, 1000);
}

function clearCountdown() {
  if (countdownInterval) clearInterval(countdownInterval);
  countdownInterval = null;
  el('countdown').classList.remove('show');
}

function updateCountdownDisplay(totalSeconds) {
  const m = String(Math.floor(totalSeconds / 60)).padStart(2, '0');
  const s = String(totalSeconds % 60).padStart(2, '0');
  el('countdownTime').textContent = `${m}:${s}`;
}

el('connectBtn').addEventListener('click', connect);
el('disconnectBtn').addEventListener('click', () => {
  if (device && device.gatt.connected) device.gatt.disconnect();
});
el('btnOn').addEventListener('click', () => { clearCountdown(); sendCommand('ON'); });
el('btnOff').addEventListener('click', () => { clearCountdown(); sendCommand('OFF'); });
document.querySelectorAll('.btn-timer').forEach(btn => {
  btn.addEventListener('click', () => {
    if (btn.dataset.secs) {
      sendCommand(btn.dataset.secs);
      startCountdownSeconds(parseInt(btn.dataset.secs));
    } else {
      const mins = btn.dataset.mins;
      sendCommand(mins);
      startCountdown(parseInt(mins));
    }
  });
});

if (!navigator.bluetooth) {
  logLine('Web Bluetooth not supported in this browser.', true);
  el('connectBtn').disabled = true;
  el('connectBtn').textContent = 'Unsupported browser';
}
</script>

</body>
</html>
