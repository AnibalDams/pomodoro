<script>
  import { onMount } from "svelte";
  let minutes = 0;
  let seconds = 0;
  let interval;
  let duration = 60 * 25;
  let _break = false;
  let breakCount = 0;
  let paused = false;
  const startTimer = () => {
    let timer = duration;
    interval = setInterval(() => {
      minutes = Math.floor(timer / 60);
      seconds = timer % 60;

      minutes = minutes < 10 ? `0${minutes}` : minutes;
      seconds = seconds < 10 ? `0${seconds}` : seconds;

      if (timer === 0) {
        if (_break === false) {
          duration = breakCount >= 4 ? 60 * 20 : 60 * 5;
          timer = breakCount >= 4 ? 60 * 20 : 60 * 5;
          _break = true;
          breakCount += 1;
        } else {
          duration = 60 * 25;
          timer = 60 * 25;
          _break = false;
        }
      }
      --timer;
      duration = timer;
    }, 1000);
  };
</script>

<div
  style={` background-color:${_break === false ? "#ff6b81" : "#5fcf57"};transition:0.5s;`}
>
  <span
    >{minutes === 0 && seconds === 0
      ? "Let's Work"
      : _break == false
        ? "Working..."
        : "Resting..."}</span
  >
  <h1 style="font-size:100px;">{minutes}:{seconds}</h1>
  <article style="display: flex;">
    <button
      style={`background-color:${_break === false ? "#721d2a" : "#285225"};transition:0.1s;`}
      on:click={() => {
        if (!paused) {
          clearInterval(interval);
          paused = true;
        } else {
          paused = false;
          startTimer();
        }
      }}>{paused ? "resume" : "pause"}</button
    >
    <button
      style={`background-color:${_break === false ? "#721d2a" : "#285225"};transition:0.1;`}
      on:click={() => {
        duration = 60 * 25;
        _break = false;
        paused = false;
        breakCount = 0
        clearInterval(interval);
        startTimer();
      }}>Start</button
    >
  </article>
</div>

<style>
  div {
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 700px;
    margin: 20px;
    border-radius: 10px;
  }
  button {
    margin-right: 10px;
    padding-left: 30px;
    padding-right: 30px;
    padding-top: 10px;
    padding-bottom: 10px;
    border: none;
    border-radius: 10px;
    font-size: 20px;
    color: white;
    font-weight: bold;
    cursor: pointer;
  }
  button:hover {
    opacity: 0.5;
  }
  span {
    font-size: 20px;
    font-weight: bold;
  }
</style>
