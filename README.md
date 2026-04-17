<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Reward Pro</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<script src="https://telegram.org/js/telegram-web-app.js"></script>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>

<style>
body {
    margin:0;
    font-family: Arial;
    background:#020617;
    color:white;
    text-align:center;
}

.container { padding:20px; }

.card {
    background:#0f172a;
    padding:20px;
    border-radius:15px;
    margin-top:15px;
}

button {
    padding:15px;
    width:100%;
    border:none;
    border-radius:10px;
    background:#22c55e;
    color:white;
    font-size:16px;
    margin-top:10px;
}
</style>
</head>

<body>

<div class="container">
    <h2>🔥 Reward Pro App</h2>

    <div class="card">
        <h3>ID: <span id="uid"></span></h3>
        <h3>Balance: <span id="balance">0</span> Coins</h3>
    </div>

    <div class="card">
        <button onclick="watchAd()">📺 Watch Ad</button>
        <button onclick="withdraw()">💸 Withdraw</button>
    </div>
</div>

<script>
// 🔥 Telegram User
let tg = window.Telegram.WebApp;
tg.expand();

let user = tg.initDataUnsafe.user;
let userId = user ? user.id : "guest";

document.getElementById("uid").innerText = userId;

// 🔥 Firebase Config (REPLACE THIS)
const firebaseConfig = {
  apiKey: "YOUR_KEY",
  authDomain: "YOUR_DOMAIN",
  projectId: "YOUR_ID"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// 🔥 Load balance
async function loadBalance() {
    let ref = db.collection("users").doc(String(userId));
    let doc = await ref.get();

    if(doc.exists){
        document.getElementById("balance").innerText = doc.data().coins;
    } else {
        await ref.set({coins:0});
    }
}
loadBalance();

// 🔥 Anti Cheat Timer
let lastClick = 0;

// 🔥 Watch Ad
function watchAd() {
    let now = Date.now();

    if(now - lastClick < 5000){
        alert("Wait 5 seconds!");
        return;
    }

    lastClick = now;

    // 👉 Replace with Adsgram
    setTimeout(async () => {

        let ref = db.collection("users").doc(String(userId));
        let doc = await ref.get();

        let current = doc.data().coins || 0;
        let updated = current + 10;

        await ref.update({coins: updated});
        document.getElementById("balance").innerText = updated;

        alert("Earned 10 coins!");

    }, 2000);
}

// 🔥 Withdraw
async function withdraw() {
    let ref = db.collection("users").doc(String(userId));
    let doc = await ref.get();

    let coins = doc.data().coins;

    if(coins < 100){
        alert("Minimum 100 coins!");
        return;
    }

    alert("Withdraw requested!");
}
</script>

</body>
</html>
