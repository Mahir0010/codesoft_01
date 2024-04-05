# codesoft_01
# alarm clock

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="alarm.css">
  <title>Alarm Clock App</title>
  <script defer src="alarmScript.js"></script>
</head>
<body>
  <div class="grid">
    <div class="neomorphic clock">
      <div id="clock">
        
      </div>
    </div>
      <div class="alarm">
        <h3>Set Alarm Time</h3>
        <div class="alarm-menu">
          <div class="neomorphic hour">
            <select id="hour-menu" class="select"></select>
          </div>
          <div class="neomorphic min">
            <select id="min-menu" class="select"></select>
          </div>
          <div class="neomorphic sec">
            <select id="sec-menu" class="select"></select>
          </div>
          <div class="neomorphic midday">
            <select id="midday-menu" class="select">
              <option value="AM">AM</option>
              <option value="PM">PM</option>
            </select>
          </div>
          <div class="set-btn">
            <button id="set-alarm" class="neomorphic btn primary">Set Alarm</button>
          </div>
          <div class="clear-btn">
            <button id="clear-alarm" class="neomorphic  btn danger">Clear Alarm</button>
          </div>
        </div>
      </div>
  </div>
</body>
</html>



<alarm.css>
/* Google font */
@import url('https://fonts.googleapis.com/css2?family=Orbitron');
@import url('https://fonts.googleapis.com/css2?family=Roboto');

*, *::before, *::after {
  box-sizing: border-box;
}

body {
  --background: hsl(222, 39%, var(--background-lightness));
  --background-lightness: 14%;
  --neumorphic-border: 7.21px 7.21px 7px #101625,
  -7.21px -7.21px 7px #1A243B;

  margin: 0; padding: 0;

  background: var(--background);
  height: 100vh;
}

.grid {
  display: grid;
  place-items: center center;
  height: 100vh;
  color: white;  
}

.neomorphic {
  box-shadow:var(--neumorphic-border);
}

.clock {
  border-radius: .5rem;
  width: 90%;
  background: linear-gradient(350deg, --background, --background);
  background-clip: border-box;
}
.alarm {
  padding: 1.5rem;
  font-family: 'Roboto', sans-serif;
  text-align: center;
}


#clock {
  padding-top: 40px;
  padding-bottom: 40px;
  
  font-family: 'Orbitron', sans-serif;
  font-size: 56px;
  text-align: center;
  z-index: 100;
}
@media (max-width: 576px) {
  #clock {
    font-size: 2em;
  }
}

.alarm-menu {
  display: grid;
  gap: 1rem;
  grid-template-areas:
    "a b c d"
    "e e f f";
  place-items: center center;
}
.alarm-menu .neomorphic {
  --neumorphic-border: 7.21px 7.21px 10px #101625,
  -7.21px -7.21px 10px #1A243B;
  --neumorphic-inset:
    inset 7.21px 7.21px 7px #101625,
    inset -7.21px -7.21px 7px #1A243B;
  border-radius: .5em;
  
}

.alarm-menu .hour {
  grid-area: a;
}
.alarm-menu .min {
  grid-area: b;
}
.alarm-menu .sec {
  grid-area: c;
}
.alarm-menu .midday {
  grid-area: d;
}
.alarm-menu .set-btn {
  grid-area: e;
  width: 100%;
}
.alarm-menu .clear-btn {
  grid-area: f;
  width: 100%;
}

.select {
  --foreground-color: white;

  border: none;

  background: none;
  color:  var(--foreground-color)
}
.select:disabled {
  --foreground-color: rgb(97, 127, 172);
}
option {
  background: #1A243B;
}

.btn {
  --foreground: #68aef0;

  margin: .675em 0;
  border: none;
  padding: .75em 1.75em;

  background: none;
  color: white;
}
.btn.primary {
  border-color: var(--foreground);
  color: var(--foreground);
}
.btn.danger {
  --foreground: #f06868;

  border-color: var(--foreground);  
  color: var(--foreground);
}
:is(.btn):active {
  box-shadow: var(--neumorphic-inset);
  color: white;
}

.alarm-menu select {
  padding: 5px 10px;
  font-size: 1em;
}
.alarm-menu button {
  width: 100%;
  font-size: 1em;
}

<alarmScript.js>

const sound = new Audio("https://www.freespecialeffects.co.uk/soundfx/sirens/analog_a.wav")
sound.loop = true
const selects = document.querySelectorAll('.alarm-menu select')

const updateTime = (time) => {
  if (time < 10) return '0' + time
  return time
}

const currentTime = () => {
  let date = new Date()  
  let hour = date.getHours()
  let min = updateTime(date.getMinutes())
  let sec = updateTime(date.getSeconds())
  let midday = (hour >= 12) ? 'PM' : 'AM'
  hour = updateTime((hour == 0) ? 12 : ((hour > 12) ? (hour - 12) : hour))
  let current_time = `${hour} : ${min} : ${sec} ${midday}`

  document.querySelector('#clock').innerText = current_time
  setTimeout(currentTime, 1000)
}

/* Alarm Menu Functions */
function SelectMenu(id, length) {
  this.select = document.getElementById(`${id}-menu`)
  this.length = length

  if (this.length == 12) for (i=1; i <= this.length; i++) select.options[select.options.length] = new Option( i < 10 ? "0" + i : i, i)
  if (this.length == 59) for (i=0; i <= this.length; i++) select.options[select.options.length] = new Option( i < 10 ? "0" + i : i, i)
}

function HourMenu(id, length){
	SelectMenu.call(this, id, length)
}

function MinMenu(id, length){
	SelectMenu.call(this, id, length)
}

function SecMenu(id, length){
	SelectMenu.call(this, id, length)
}

const setAlarm = () => {
  const alarm_time =
    `${updateTime(document.querySelector('#hour-menu').value)} : ${updateTime(document.querySelector('#min-menu').value)} : ${updateTime(document.querySelector('#sec-menu').value)} ${document.querySelector('#midday-menu').value}`

  selects.forEach((select) => select.disabled = true)
  setInterval(() => {
    let current_time = document.querySelector('#clock').innerText
    if (current_time == alarm_time) sound.play()
  }, 1000)
}

const clearAlarm = () => {
  sound.pause()
  selects.forEach((select) => select.disabled = false)
}
/* Alarm Menu Functions */

HourMenu('hour', 12)
MinMenu('min', 59)
SecMenu('sec', 59)
document.querySelector('#set-alarm').addEventListener('click', setAlarm)
document.querySelector('#clear-alarm').addEventListener('click', clearAlarm)
currentTime()

