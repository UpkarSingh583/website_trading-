<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>TradingView Chart with Live Data</title>
<style>
  body, html {
    margin: 0; padding: 0; height: 100%;
    font-family: Arial, sans-serif;
    background-color: #fff;
    display: flex; flex-direction: column;
  }
  header {
    background: #1a73e8; color: white; padding: 15px 20px; text-align: center;
  }
  header h1 {
    margin: 0; font-weight: 700; font-size: 24px;
  }
  #controls {
    background: #f5f5f5;
    display: flex; flex-wrap: wrap; justify-content: center;
    gap: 10px; padding: 10px 20px;
    align-items: center;
  }
  label {
    font-size: 14px;
  }
  input[type="text"], select {
    padding: 7px 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    width: 170px;
  }
  button {
    padding: 7px 15px;
    font-size: 14px;
    border-radius: 4px;
    border: none;
    cursor: pointer;
    background-color: #1a73e8;
    color: white;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #155ab6;
  }
  #tv_chart_container {
    flex-grow: 1;
    min-height: 600px;
  }
  footer {
    background: #1a73e8; color: white; text-align: center;
    padding: 12px 20px; font-size: 14px; user-select: none;
  }
</style>
</head>
<body>

<header>
  <h1>TradingView Chart with Live Data</h1>
</header>

<div id="controls">
  <label for="symbolInput">Symbol:</label>
  <input type="text" id="symbolInput" placeholder="E.g. BINANCE:BTCUSDT" list="symbolsList" autocomplete="off" />

  <datalist id="symbolsList">
    <option value="NSE:NIFTY"></option>
    <option value="NSE:RELIANCE"></option>
    <option value="NASDAQ:AAPL"></option>
    <option value="NASDAQ:GOOGL"></option>
    <option value="NYSE:TSLA"></option>
    <option value="BINANCE:BTCUSDT"></option>
    <option value="BINANCE:ETHUSDT"></option>
    <option value="BINANCE:BNBUSDT"></option>
    <option value="BINANCE:ADAUSDT"></option>
    <option value="BINANCE:XRPUSDT"></option>
  </datalist>

  <label for="intervalSelect">Interval:</label>
  <select id="intervalSelect" title="Select Interval">
    <option value="1">1 Minute</option>
    <option value="5" selected>5 Minutes</option>
    <option value="15">15 Minutes</option>
    <option value="30">30 Minutes</option>
    <option value="60">1 Hour</option>
    <option value="240">4 Hours</option>
    <option value="D">1 Day</option>
    <option value="W">1 Week</option>
    <option value="M">1 Month</option>
  </select>

  <label for="styleSelect">Chart Style:</label>
  <select id="styleSelect" title="Select Chart Style">
    <option value="0">Bars</option>
    <option value="1" selected>Candles</option>
    <option value="2">Line</option>
    <option value="3">Area</option>
    <option value="4">Heikin Ashi</option>
    <option value="5">Hollow Candles</option>
  </select>

  <button id="themeToggleBtn">Switch to Dark Theme</button>
  <button id="applyBtn">Apply</button>
</div>

<div id="tv_chart_container"></div>

<footer>
  &copy; 2025 YourWebsiteName. All rights reserved.
</footer>

<script src="https://s3.tradingview.com/tv.js"></script>
<script>
  let widget = null;
  let currentTheme = localStorage.getItem("theme") || "light";

  const symbolInput = document.getElementById("symbolInput");
  const intervalSelect = document.getElementById("intervalSelect");
  const styleSelect = document.getElementById("styleSelect");
  const themeToggleBtn = document.getElementById("themeToggleBtn");
  const applyBtn = document.getElementById("applyBtn");

  // Set default values or from localStorage
  symbolInput.value = localStorage.getItem("symbol") || "BINANCE:BTCUSDT";
  intervalSelect.value = localStorage.getItem("interval") || "5";  // default 5 minutes
  styleSelect.value = localStorage.getItem("style") || "1";        // default candles

  function createWidget() {
    if (widget) {
      document.getElementById("tv_chart_container").innerHTML = "";
    }
    widget = new TradingView.widget({
      container_id: "tv_chart_container",
      autosize: true,
      symbol: symbolInput.value || "BINANCE:BTCUSDT",
      interval: intervalSelect.value,
      timezone: "Asia/Kolkata",
      theme: currentTheme,
      style: parseInt(styleSelect.value, 10),
      locale: "en",
      toolbar_bg: currentTheme === "dark" ? "#222" : "#f1f3f6",
      enable_publishing: false,
      allow_symbol_change: true,
      hideideas: true,
      studies: ["MACD@tv-basicstudies", "RSI@tv-basicstudies"],
      withdateranges: true,
      hide_side_toolbar: false,
      details: true,
      hotlist: true,
      calendar: true,
      watchlist: [
        "BINANCE:BTCUSDT",
        "BINANCE:ETHUSDT",
        "NASDAQ:AAPL",
        "NYSE:TSLA",
        "NSE:NIFTY"
      ],
    });

    localStorage.setItem("symbol", symbolInput.value);
    localStorage.setItem("interval", intervalSelect.value);
    localStorage.setItem("style", styleSelect.value);
    localStorage.setItem("theme", currentTheme);
  }

  function updateThemeButton() {
    themeToggleBtn.textContent = currentTheme === "light" ? "Switch to Dark Theme" : "Switch to Light Theme";
    if (currentTheme === "dark") {
      document.body.style.backgroundColor = "#121212";
      document.body.style.color = "#eee";
    } else {
      document.body.style.backgroundColor = "#fff";
      document.body.style.color = "#000";
    }
  }

  themeToggleBtn.addEventListener("click", () => {
    currentTheme = currentTheme === "light" ? "dark" : "light";
    updateThemeButton();
    createWidget();
  });

  applyBtn.addEventListener("click", () => {
    createWidget();
  });

  // Initialize theme and widget
  updateThemeButton();
  createWidget();
</script>

</body>
</html>
