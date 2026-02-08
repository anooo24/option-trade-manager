let balance = 0;
let targetBalance = 0;
let tradingLocked = false;
let consecutiveLosses = 0;

const RISK_PERCENT = 2;
const PAYOUT = 0.85;
const MIN_INVEST = 1;
const MVR_RATE = 15.42;
const BODUBERU_URL = "https://www.youtube.com/results?search_query=boduberu+music";

// ===== START =====
function start() {
    balance = Number(document.getElementById("capital").value);
    targetBalance = Number(document.getElementById("target").value);
    tradingLocked = false;
    consecutiveLosses = 0;

    document.getElementById("targetShow").innerText = targetBalance.toFixed(2);
    document.getElementById("status").innerText = "";
    unlockButtons();
    updateUI();
}

// ===== INVEST CALC =====
function calculateInvestment() {
    const risk = balance * (RISK_PERCENT / 100);
    return risk < MIN_INVEST ? MIN_INVEST : risk;
}

// ===== UPDATE UI =====
function updateUI() {
    const invest = calculateInvestment();
    const profit = invest * PAYOUT;

    document.getElementById("invest").innerText = invest.toFixed(2);
    document.getElementById("loss").innerText = invest.toFixed(2);
    document.getElementById("profit").innerText = profit.toFixed(2);
    document.getElementById("balanceUSD").innerText = balance.toFixed(2);
    document.getElementById("balanceMVR").innerText = (balance * MVR_RATE).toFixed(2);
    document.getElementById("note").innerText = invest === MIN_INVEST ? "⚠ Minimum $1 applied" : "";
    checkTarget();
}

// ===== TARGET CHECK =====
function checkTarget() {
    if (balance >= targetBalance && targetBalance > 0) {
        tradingLocked = true;
        lockButtons();
        const motivation = getMotivation();
        document.getElementById("status").innerText = `🎯 TARGET HIT! — ${motivation}`;
        document.getElementById("status").style.color = "#22c55e";
    }
}

// ===== MOTIVATION =====
const motivationalQuotes = [
    "Great job! Keep up the disciplined trading! 💪",
    "You did it! Take a break and enjoy your success! 🌟",
    "Fantastic! Remember, patience is the key to consistent profits! 🧠",
    "Awesome! Celebrate the win, but stay humble and focused! 🎯",
    "You reached your goal! One step closer to mastery! 🚀"
];

function getMotivation() {
    const index = Math.floor(Math.random() * motivationalQuotes.length);
    return motivationalQuotes[index];
}

// ===== WIN =====
function win() {
    if (tradingLocked) return;
    const invest = calculateInvestment();
    balance += invest * PAYOUT;
    consecutiveLosses = 0;
    updateUI();
}

// ===== LOSS =====
function loss() {
    if (tradingLocked) return;
    const invest = calculateInvestment();
    balance -= invest;
    if (balance < 0) balance = 0;
    consecutiveLosses++;
    updateUI();
    if (consecutiveLosses >= 2) {
        document.getElementById("status").innerText = "🧠 2 LOSSES — RELAX WITH BODUBERU";
        document.getElementById("status").style.color = "#fbbf24";
        window.open(BODUBERU_URL, "_blank");
        consecutiveLosses = 0;
    }
}

// ===== BUTTON CONTROL =====
function lockButtons() {
    document.getElementById("winBtn").disabled = true;
    document.getElementById("lossBtn").disabled = true;
}
function unlockButtons() {
    document.getElementById("winBtn").disabled = false;
    document.getElementById("lossBtn").disabled = false;
}

// ===== TIPS =====
const tips = [
    "📌 Follow the system, not emotions.",
    "🛑 Stop when target is reached.",
    "❌ Never revenge trade.",
    "⚠ Avoid high-impact news.",
    "🧠 Two losses? Reset your mind.",
    "🎵 Music helps emotional balance.",
    "💰 Protect capital first.",
    "⏱ Quality trades only.",
    "🎯 One target per session.",
    "🧘 Calm mind = better trades."
];
let tipIndex = 0;
setInterval(() => {
    document.getElementById("tipText").innerText = tips[tipIndex];
    tipIndex = (tipIndex + 1) % tips.length;
}, 5000);

// ===== MARKET ANALYSIS =====
function openAnalysis(type) {
    let url = "";
    switch(type) {
        case "tradingview": url = "https://www.tradingview.com/markets/"; break;
        case "news": url = "https://www.google.com/search?q=market+news+forex+stocks+crypto"; break;
        case "calendar": url = "https://www.investing.com/economic-calendar/"; break;
        case "sentiment": url = "https://www.google.com/search?q=market+sentiment+analysis"; break;
    }
    if (url) window.open(url, "_blank");
}

// ===== EDUCATION TAB =====
let eduOpen = false;
function toggleEducation() {
    const edu = document.getElementById("education");
    eduOpen = !eduOpen;
    if (eduOpen) {
        edu.classList.add("open");
    } else {
        edu.classList.remove("open");
    }
}
