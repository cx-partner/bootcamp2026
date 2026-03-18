<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Webex Bank Rewards Portal</title>

<style>
body {
  margin: 0;
  font-family: "Segoe UI", Roboto, sans-serif;
  background: #f4f7fb;
  color: #1a1a1a;
}

/* NAVBAR */
.navbar {
  position: sticky;
  top: 0;
  background: rgba(11, 61, 145, 0.95);
  backdrop-filter: blur(10px);
  color: white;
  padding: 15px 40px;
  display: flex;
  justify-content: space-between;
}

.navbar a {
  color: white;
  margin-left: 20px;
  text-decoration: none;
  opacity: 0.9;
}

/* HERO */
.hero {
  background: linear-gradient(135deg, #0b3d91, #1e90ff);
  color: white;
  padding: 80px 20px;
  text-align: center;
}

/* CONTAINER */
.container {
  max-width: 1100px;
  margin: auto;
  padding: 30px;
}

/* CARDS */
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
  transition: 0.3s;
  box-shadow: 0 6px 20px rgba(0,0,0,0.08);
}

.card:hover {
  transform: translateY(-10px);
}

/* SECTION */
.section {
  margin-top: 30px;
  background: white;
  padding: 30px;
  border-radius: 16px;
}

/* TABS */
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

.footer {
  text-align: center;
  padding: 40px;
  color: #777;
}
</style>
</head>

<body>

<div class="navbar">
  <div><strong>Webex Bank</strong></div>
  <div>
    <a href="#">Accounts</a>
    <a href="#">Cards</a>
    <a href="#">Rewards</a>
    <a href="#">Support</a>
  </div>
</div>

<div class="hero">
  <h1>Rewards That Move With You</h1>
  <p>Earn more from every purchase with flexible rewards and premium benefits.</p>
</div>

<div class="container">

<!-- CARDS (UNCHANGED) -->
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
      Includes <strong>loyalty boost up to +50%</strong>.
    </p>
  </div>

</div>

<!-- TERMS -->
<div class="section">
  <h2>Rewards Program Terms & Conditions</h2>

  <div class="tabs">
    <div class="tab active" onclick="openTab(event, 'earning')">Earning</div>
    <div class="tab" onclick="openTab(event, 'redemption')">Redemption</div>
    <div class="tab" onclick="openTab(event, 'expiration')">Expiration</div>
    <div class="tab" onclick="openTab(event, 'account')">Account</div>
    <div class="tab" onclick="openTab(event, 'limitations')">Limitations</div>
    <div class="tab" onclick="openTab(event, 'security')">Security</div>
  </div>

  <div id="earning" class="tab-content active">
    <p>
      Rewards are earned on most standard purchase transactions, including dining, retail, groceries, and travel. 
      The earning rate depends on your card type and purchase category.
    </p>
    <p>
      Transactions such as cash advances, balance transfers, interest charges, fees, and refunded purchases are excluded.
    </p>
    <p>
      Webex Bank may introduce promotional categories or partner offers with enhanced earning rates.
    </p>
  </div>

  <div id="redemption" class="tab-content">
    <p>
      Rewards hold no value until redeemed. Cashback is typically applied as statement credit, 
      while travel rewards may be redeemed for flights and hotels.
    </p>
    <p>
      Rewards are personal and cannot be transferred or sold.
    </p>
  </div>

  <div id="expiration" class="tab-content">
    <p>
      Rewards are valid between two and five years depending on the card type.
    </p>
    <p>
      Rewards expire if unused and may be forfeited due to account closure or fraud.
    </p>
  </div>

  <div id="account" class="tab-content">
    <p>
      Accounts must remain active and in good standing to earn and redeem rewards.
    </p>
    <p>
      Delinquent or suspended accounts may lose reward privileges.
    </p>
  </div>

  <div id="limitations" class="tab-content">
    <p>
      Rewards cannot be exchanged directly for cash. Returned purchases reverse earned rewards.
    </p>
    <p>
      Some benefits may be limited to specific merchants or categories.
    </p>
  </div>

  <div id="security" class="tab-content">
    <p>
      Fraudulent or abusive activity disqualifies rewards and may result in forfeiture.
    </p>
    <p>
      Webex Bank reserves the right to modify the program at any time.
    </p>
  </div>

</div>

<!-- FINAL NOTES -->
<div class="section">
  <h2>Final Notes</h2>
  <p>
    The Webex Bank Rewards Program is designed to deliver long-term value and flexibility.
  </p>
  <p>
    Customers are encouraged to review full terms or contact support for additional details.
  </p>
</div>

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