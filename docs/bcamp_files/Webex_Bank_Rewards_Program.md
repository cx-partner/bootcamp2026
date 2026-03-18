<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Webex Bank Rewards Portal</title>

<style>

/* ===== GLOBAL ===== */
body {
  margin: 0;
  font-family: "Segoe UI", Roboto, sans-serif;
  background: #f4f7fb;
  color: #1a1a1a;
}

/* ===== NAVBAR ===== */
.navbar {
  position: sticky;
  top: 0;
  background: rgba(11, 61, 145, 0.95);
  backdrop-filter: blur(10px);
  color: white;
  padding: 15px 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 1000;
}

.navbar h2 {
  margin: 0;
  font-weight: 600;
}

.navbar a {
  color: white;
  margin-left: 20px;
  text-decoration: none;
  font-size: 14px;
  opacity: 0.9;
  transition: 0.3s;
}

.navbar a:hover {
  opacity: 1;
  transform: translateY(-2px);
}

/* ===== HERO ===== */
.hero {
  background: linear-gradient(135deg, #0b3d91, #1e90ff);
  color: white;
  padding: 80px 20px;
  text-align: center;
  animation: fadeIn 1s ease-in-out;
}

.hero h1 {
  font-size: 2.8em;
  margin-bottom: 10px;
}

.hero p {
  max-width: 600px;
  margin: auto;
  opacity: 0.9;
}

/* ===== CONTAINER ===== */
.container {
  max-width: 1100px;
  margin: auto;
  padding: 30px;
}

/* ===== CARDS ===== */
.cards {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.card {
  flex: 1;
  min-width: 280px;
  background: white;
  padding: 25px;
  border-radius: 16px;
  transition: all 0.3s ease;
  box-shadow: 0 6px 20px rgba(0,0,0,0.08);
  position: relative;
  overflow: hidden;
}

.card::before {
  content: "";
  position: absolute;
  width: 100%;
  height: 4px;
  top: 0;
  left: 0;
  background: linear-gradient(90deg, #1e90ff, #0b3d91);
}

.card:hover {
  transform: translateY(-10px) scale(1.03);
  box-shadow: 0 15px 35px rgba(0,0,0,0.15);
}

/* ===== SECTIONS ===== */
.section {
  margin-top: 30px;
  background: white;
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.05);
  animation: fadeUp 0.6s ease;
}

/* ===== TABS ===== */
.tabs {
  display: flex;
  margin-bottom: 15px;
}

.tab {
  padding: 10px 20px;
  cursor: pointer;
  background: #e9effa;
  margin-right: 10px;
  border-radius: 8px;
  transition: 0.3s;
}

.tab.active {
  background: #0b3d91;
  color: white;
}

.tab-content {
  display: none;
}

.tab-content.active {
  display: block;
}

/* ===== FOOTER ===== */
.footer {
  text-align: center;
  padding: 40px;
  font-size: 0.9em;
  color: #777;
}

/* ===== ANIMATIONS ===== */
@keyframes fadeIn {
  from {opacity: 0;}
  to {opacity: 1;}
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

</style>
</head>

<body>

<!-- NAVBAR -->
<div class="navbar">
  <h2>Webex Bank</h2>
  <div>
    <a href="#">Accounts</a>
    <a href="#">Cards</a>
    <a href="#">Rewards</a>
    <a href="#">Support</a>
  </div>
</div>

<!-- HERO -->
<div class="hero">
  <h1>Rewards That Move With You</h1>
  <p>Earn more from every purchase with flexible rewards, premium benefits, and total transparency.</p>
</div>

<div class="container">

<!-- CARDS -->
<h2>Our Cards</h2>
<div class="cards">

  <div class="card">
    <h3>🟢 Standard Cashback</h3>
    <p><strong>Simple, powerful everyday rewards.</strong></p>
    <p>
      Earn <strong>1.5% unlimited cashback</strong> on all purchases, with 
      <strong>up to 3% in categories</strong> like groceries and fuel.
    </p>
    <p>
      Cashback is automatically applied as statement credit with no minimum redemption threshold.
    </p>
  </div>

  <div class="card">
    <h3>✈️ Travel Rewards</h3>
    <p><strong>Turn spending into experiences.</strong></p>
    <p>
      Earn <strong>2x points on all purchases</strong> and up to 
      <strong>5x on travel bookings</strong>.
    </p>
    <p>
      Redeem for flights, hotels, or cashback with flexible options.
    </p>
  </div>

  <div class="card">
    <h3>🏆 Platinum</h3>
    <p><strong>Premium rewards. Maximum acceleration.</strong></p>
    <p>
      Earn <strong>2% base cashback</strong> plus up to 
      <strong>5% bonus categories</strong>.
    </p>
    <p>
      Includes <strong>loyalty boost up to +50%</strong> based on relationship value.
    </p>
  </div>

</div>

<!-- LOYALTY -->
<div class="section">
  <h2>Loyalty Boost Program</h2>
  <p>
    Webex Bank rewards long-term relationships. Customers with higher balances or multiple products 
    receive bonus rewards between <strong>10% and 50%</strong>.
  </p>
</div>

<!-- TERMS -->
<div class="section">
  <h2>Rewards Program Terms & Conditions</h2>

  <div class="tabs">
    <div class="tab active" onclick="openTab(event, 'earning')">Earning</div>
    <div class="tab" onclick="openTab(event, 'redemption')">Redemption</div>
    <div class="tab" onclick="openTab(event, 'expiration')">Expiration</div>
  </div>

  <div id="earning" class="tab-content active">
    <p>
      Earn rewards on eligible purchases with base rates from 1.5% to 2%, 
      and up to 5% in bonus categories.
    </p>
    <p>
      Exclusions include cash advances, fees, and refunds.
    </p>
  </div>

  <div id="redemption" class="tab-content">
    <p>
      Redeem rewards for cashback, travel, or partner services. 
      Rewards have no value until redeemed and cannot be transferred.
    </p>
  </div>

  <div id="expiration" class="tab-content">
    <p>
      Rewards expire between 24–60 months.  
      Accounts in bad standing or fraud cases may forfeit rewards.
    </p>
  </div>

</div>

<!-- SECURITY -->
<div class="section">
  <h2>Security & Trust</h2>
  <p>
    Advanced fraud detection ensures secure transactions. Suspicious activity may result in 
    reward forfeiture or account restrictions.
  </p>
</div>

<!-- FOOTER -->
<div class="footer">
  © 2026 Webex Bank — Simulated Demo Environment
</div>

</div>

<script>
function openTab(evt, tabName) {
  let i, tabcontent, tablinks;

  tabcontent = document.getElementsByClassName("tab-content");
  for (i = 0; i < tabcontent.length; i++) {
    tabcontent[i].classList.remove("active");
  }

  tablinks = document.getElementsByClassName("tab");
  for (i = 0; i < tablinks.length; i++) {
    tablinks[i].classList.remove("active");
  }

  document.getElementById(tabName).classList.add("active");
  evt.currentTarget.classList.add("active");
}
</script>

</body>
</html>