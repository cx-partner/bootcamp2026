<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Webex Bank Rewards Portal</title>

<style>
  :root {
    --navy: #0b3d91;
    --blue: #1e90ff;
    --light-blue: #e9effa;
    --gold: #f5a623;
    --platinum: #6c7a89;
    --green: #27ae60;
    --white: #ffffff;
    --bg: #f4f7fb;
    --text: #1a1a1a;
    --muted: #666;
    --card-shadow: 0 6px 24px rgba(0,0,0,0.09);
    --radius: 16px;
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    font-family: "Segoe UI", Roboto, sans-serif;
    background: var(--bg);
    color: var(--text);
  }

  /* NAVBAR */
  .navbar {
    position: sticky;
    top: 0;
    background: rgba(11, 61, 145, 0.97);
    backdrop-filter: blur(10px);
    color: white;
    padding: 15px 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 999;
  }
  .navbar .brand { font-size: 1.2rem; font-weight: 700; letter-spacing: 0.5px; }
  .navbar a {
    color: white;
    margin-left: 20px;
    text-decoration: none;
    opacity: 0.85;
    font-size: 0.95rem;
    transition: opacity 0.2s;
  }
  .navbar a:hover { opacity: 1; }

  /* HERO */
  .hero {
    background: linear-gradient(135deg, #0b3d91, #1565c0, #1e90ff);
    color: white;
    padding: 90px 20px 70px;
    text-align: center;
  }
  .hero h1 { font-size: 2.6rem; margin-bottom: 12px; }
  .hero p { font-size: 1.15rem; opacity: 0.9; margin: 0 auto; max-width: 600px; }
  .hero-badges {
    display: flex;
    justify-content: center;
    gap: 14px;
    margin-top: 28px;
    flex-wrap: wrap;
  }
  .hero-badge {
    background: rgba(255,255,255,0.18);
    border: 1px solid rgba(255,255,255,0.35);
    border-radius: 30px;
    padding: 8px 18px;
    font-size: 0.88rem;
    backdrop-filter: blur(6px);
  }

  /* CONTAINER */
  .container {
    max-width: 1100px;
    margin: auto;
    padding: 40px 30px;
  }

  /* SECTION TITLE */
  .section-title {
    font-size: 1.55rem;
    font-weight: 700;
    color: var(--navy);
    margin-bottom: 6px;
  }
  .section-subtitle {
    color: var(--muted);
    margin-bottom: 24px;
    font-size: 0.97rem;
  }

  /* CARDS GRID */
  .cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 22px;
  }

  .card {
    background: white;
    padding: 28px;
    border-radius: var(--radius);
    box-shadow: var(--card-shadow);
    transition: transform 0.25s, box-shadow 0.25s;
    display: flex;
    flex-direction: column;
    border-top: 4px solid transparent;
  }
  .card:hover {
    transform: translateY(-8px);
    box-shadow: 0 16px 36px rgba(0,0,0,0.14);
  }
  .card.standard { border-top-color: var(--green); }
  .card.travel   { border-top-color: var(--blue);  }
  .card.platinum { border-top-color: var(--gold);  }

  .card-header { display: flex; align-items: center; gap: 12px; margin-bottom: 14px; }
  .card-icon { font-size: 2rem; }
  .card-title { font-size: 1.15rem; font-weight: 700; margin: 0; }
  .card-tagline { font-size: 0.82rem; color: var(--muted); margin: 0; }

  .card-badge {
    display: inline-block;
    font-size: 0.78rem;
    border-radius: 6px;
    padding: 3px 10px;
    font-weight: 600;
    margin-bottom: 10px;
  }
  .badge-free  { background: #eafaf1; color: var(--green); }
  .badge-mid   { background: #e8f4fd; color: #1565c0; }
  .badge-prem  { background: #fdf6e3; color: #b7860b; }

  .card-stat {
    background: var(--bg);
    border-radius: 10px;
    padding: 10px 14px;
    margin: 8px 0;
    font-size: 0.9rem;
  }
  .card-stat strong { color: var(--navy); }

  .card-divider { border: none; border-top: 1px solid #eee; margin: 14px 0; }

  .card-bonus {
    background: linear-gradient(90deg, #fff8e1, #fff3cd);
    border-left: 3px solid var(--gold);
    border-radius: 8px;
    padding: 10px 14px;
    font-size: 0.88rem;
    margin-top: 10px;
  }
  .card-bonus strong { color: #7d5800; }

  .card-perks { list-style: none; padding: 0; margin: 10px 0 0; }
  .card-perks li { font-size: 0.88rem; padding: 4px 0; color: var(--muted); }
  .card-perks li::before { content: "✓ "; color: var(--green); font-weight: 700; }

  /* RATES SECTION */
  .rates-section {
    margin-top: 40px;
    background: white;
    border-radius: var(--radius);
    box-shadow: var(--card-shadow);
    overflow: hidden;
  }
  .rates-header {
    background: linear-gradient(90deg, var(--navy), #1565c0);
    color: white;
    padding: 20px 28px;
  }
  .rates-header h2 { margin: 0; font-size: 1.2rem; }
  .rates-header p  { margin: 4px 0 0; opacity: 0.85; font-size: 0.9rem; }
  .rates-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 0;
  }
  .rates-item {
    padding: 20px 24px;
    border-right: 1px solid #f0f0f0;
    border-bottom: 1px solid #f0f0f0;
  }
  .rates-item:last-child { border-right: none; }
  .rates-label { font-size: 0.82rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.5px; }
  .rates-value { font-size: 1.2rem; font-weight: 700; color: var(--navy); margin-top: 4px; }
  .rates-note  { font-size: 0.78rem; color: var(--muted); margin-top: 2px; }

  /* TIERS */
  .tiers {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(290px, 1fr));
    gap: 22px;
    margin-top: 24px;
  }
  .tier-card {
    background: white;
    border-radius: var(--radius);
    box-shadow: var(--card-shadow);
    overflow: hidden;
    transition: transform 0.25s;
  }
  .tier-card:hover { transform: translateY(-6px); }

  .tier-header {
    padding: 20px 24px;
    color: white;
    font-weight: 700;
    font-size: 1.15rem;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .tier-basic    .tier-header { background: linear-gradient(90deg, #2c3e50, #4a627a); }
  .tier-preferred .tier-header { background: linear-gradient(90deg, #1565c0, #1e90ff); }
  .tier-elite    .tier-header { background: linear-gradient(90deg, #7d5800, #f5a623); }

  .tier-body { padding: 20px 24px; }
  .tier-qualify {
    font-size: 0.85rem;
    color: var(--muted);
    background: var(--bg);
    border-radius: 8px;
    padding: 8px 12px;
    margin-bottom: 14px;
  }
  .tier-qualify strong { color: var(--text); }
  .tier-perks { list-style: none; padding: 0; margin: 0; }
  .tier-perks li {
    font-size: 0.9rem;
    padding: 6px 0;
    border-bottom: 1px solid #f5f5f5;
    display: flex;
    align-items: flex-start;
    gap: 8px;
  }
  .tier-perks li:last-child { border-bottom: none; }
  .tier-perks .perk-icon { flex-shrink: 0; }

  /* AIRLINE PARTNERS */
  .partners-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 16px;
    margin-top: 20px;
  }
  .partner-card {
    background: white;
    border-radius: var(--radius);
    padding: 20px 22px;
    box-shadow: var(--card-shadow);
    border-left: 4px solid var(--blue);
  }
  .partner-name { font-weight: 700; font-size: 1rem; margin-bottom: 6px; }
  .partner-benefit { font-size: 0.9rem; color: var(--muted); }

  /* TABS */
  .section-block {
    margin-top: 40px;
    background: white;
    border-radius: var(--radius);
    box-shadow: var(--card-shadow);
    overflow: hidden;
  }
  .section-block-header {
    padding: 22px 28px 0;
    border-bottom: 1px solid #f0f0f0;
  }
  .section-block-header h2 { margin: 0 0 16px; font-size: 1.3rem; color: var(--navy); }

  .tabs {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
    padding-bottom: 0;
  }
  .tab {
    padding: 9px 18px;
    cursor: pointer;
    background: var(--light-blue);
    border-radius: 8px 8px 0 0;
    font-size: 0.88rem;
    font-weight: 600;
    color: var(--navy);
    transition: background 0.2s, color 0.2s;
    border: none;
  }
  .tab:hover { background: #d0dff7; }
  .tab.active { background: var(--navy); color: white; }

  .tab-content { display: none; padding: 24px 28px; }
  .tab-content.active { display: block; }
  .tab-content p { line-height: 1.65; color: #444; }
  .tab-content ul { padding-left: 20px; }
  .tab-content ul li { margin-bottom: 6px; color: #444; line-height: 1.55; }

  /* FAQ */
  .faq-list { margin-top: 20px; }
  .faq-item {
    background: white;
    border-radius: 12px;
    box-shadow: 0 3px 12px rgba(0,0,0,0.06);
    margin-bottom: 12px;
    overflow: hidden;
  }
  .faq-question {
    padding: 16px 22px;
    font-weight: 600;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: var(--navy);
    user-select: none;
  }
  .faq-question:hover { background: var(--light-blue); }
  .faq-arrow { font-size: 1rem; transition: transform 0.2s; }
  .faq-item.open .faq-arrow { transform: rotate(180deg); }
  .faq-answer {
    display: none;
    padding: 0 22px 16px;
    color: #555;
    line-height: 1.65;
    font-size: 0.95rem;
  }
  .faq-item.open .faq-answer { display: block; }

  /* CONTACT */
  .contact-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 18px;
    margin-top: 20px;
  }
  .contact-card {
    background: white;
    border-radius: var(--radius);
    padding: 22px;
    box-shadow: var(--card-shadow);
    text-align: center;
  }
  .contact-icon { font-size: 2rem; margin-bottom: 10px; }
  .contact-label { font-size: 0.82rem; text-transform: uppercase; letter-spacing: 0.5px; color: var(--muted); }
  .contact-value { font-weight: 700; font-size: 0.97rem; color: var(--navy); margin-top: 4px; }
  .contact-note  { font-size: 0.8rem; color: var(--muted); margin-top: 4px; }

  .footer {
    text-align: center;
    padding: 40px;
    color: #999;
    font-size: 0.88rem;
    margin-top: 20px;
  }

  /* RESPONSIVE */
  @media (max-width: 600px) {
    .hero h1 { font-size: 1.8rem; }
    .navbar { padding: 14px 20px; }
    .container { padding: 30px 16px; }
  }

  .brand .logo {
  height: 40px;      /* Adjust this height to match your navbar size */
  width: auto;       /* Maintains the aspect ratio */
  display: block;    /* Removes extra whitespace below the image */
  }

  /* Ensure the brand div centers the logo if needed */
  .brand {
    display: flex;
    align-items: center;
  }
</style>
</head>

<body>

<div class="navbar">
  <div class="brand">
    <img src="../logo1.png" alt="Webex Bank" class="logo" height="40">
    Webex Bank
  </div>
  <div>
    <a href="#cards">Cards</a>
    <a href="#rates">Rates</a>
    <a href="#tiers">Rewards</a>
    <a href="#faq">FAQ</a>
    <a href="#support">Support</a>
  </div>
</div>

<!-- HERO -->
<div class="hero">
  <h1>Rewards That Move With You</h1>
  <p>Earn more from every purchase with flexible rewards, elite tiers, and premium travel benefits.</p>
  <div class="hero-badges">
    <div class="hero-badge">💳 No Hidden Fees on Standard Card</div>
    <div class="hero-badge">✈️ 3 Airline Partners</div>
    <div class="hero-badge">🛡️ Zero Liability Fraud Protection</div>
    <div class="hero-badge">🏆 Elite Tier Perks</div>
  </div>
</div>

<div class="container">

  <!-- CARDS -->
  <div id="cards">
    <div class="section-title">Our Credit Cards</div>
    <div class="section-subtitle">Choose the card that fits your lifestyle — from everyday cashback to premium travel rewards.</div>

    <div class="cards">

      <!-- STANDARD -->
      <div class="card standard">
        <div class="card-header">
          <div class="card-icon">🟢</div>
          <div>
            <p class="card-title">Standard Cashback</p>
            <p class="card-tagline">Everyday spending, zero annual fee</p>
          </div>
        </div>
        <span class="card-badge badge-free">$0 Annual Fee</span>

        <div class="card-stat">🍽️ Dining — <strong>3% cashback</strong></div>
        <div class="card-stat">🛒 Groceries — <strong>2% cashback</strong></div>
        <div class="card-stat">🛍️ All Other Purchases — <strong>1% cashback</strong></div>

        <div class="card-bonus">
          <strong>Sign-Up Bonus:</strong> Earn $200 cashback after spending $1,000 in the first 3 months.
        </div>

        <hr class="card-divider">
        <ul class="card-perks">
          <li>Cashback applied as statement credit automatically</li>
          <li>Enhanced rates for Preferred &amp; Elite members</li>
          <li>No minimum redemption threshold</li>
          <li>0% intro APR for first 12 months</li>
        </ul>
      </div>

      <!-- TRAVEL -->
      <div class="card travel">
        <div class="card-header">
          <div class="card-icon">✈️</div>
          <div>
            <p class="card-title">Travel Rewards</p>
            <p class="card-tagline">Turn spending into experiences</p>
          </div>
        </div>
        <span class="card-badge badge-mid">$95 Annual Fee</span>

        <div class="card-stat">✈️ Travel &amp; Dining — <strong>2 miles per $1</strong></div>
        <div class="card-stat">🌍 All Other Purchases — <strong>1 mile per $1</strong></div>
        <div class="card-stat">🏖️ Redeem for flights, hotels, or cashback</div>

        <div class="card-bonus">
          <strong>Sign-Up Bonus:</strong> Earn 50,000 miles after spending $3,000 in the first 3 months.
        </div>

        <hr class="card-divider">
        <ul class="card-perks">
          <li>Delta: 10% discount on tickets booked with rewards</li>
          <li>United: Free checked bag + priority boarding</li>
          <li>American: Earn 25% more miles on AA purchases</li>
          <li>Flexible redemption — flights, hotels, or cashback</li>
        </ul>
      </div>

      <!-- PLATINUM -->
      <div class="card platinum">
        <div class="card-header">
          <div class="card-icon">🏆</div>
          <div>
            <p class="card-title">Platinum Card</p>
            <p class="card-tagline">Premium rewards &amp; maximum acceleration</p>
          </div>
        </div>
        <span class="card-badge badge-prem">$250 Annual Fee</span>

        <div class="card-stat">🌍 Travel — <strong>3 points per $1</strong></div>
        <div class="card-stat">🍽️ Dining — <strong>2 points per $1</strong></div>
        <div class="card-stat">🛍️ Everything Else — <strong>1 point per $1</strong></div>

        <div class="card-bonus">
          <strong>Sign-Up Bonus:</strong> Earn 80,000 points after spending $5,000 in the first 3 months.
        </div>

        <hr class="card-divider">
        <ul class="card-perks">
          <li>Airport lounge access at 1,200+ locations worldwide</li>
          <li>Complimentary TSA PreCheck / Global Entry credit</li>
          <li>Dedicated concierge service (Elite members)</li>
          <li>Loyalty boost up to +50% for Elite tier</li>
          <li>Annual $50 statement credit (Elite, travel/dining)</li>
        </ul>
      </div>

    </div>
  </div>

  <!-- RATES & FEES -->
  <div id="rates" class="rates-section">
    <div class="rates-header">
      <h2>Interest Rates &amp; Key Fees</h2>
      <p>Transparent pricing across all Webex Bank credit cards</p>
    </div>
    <div class="rates-grid">
      <div class="rates-item">
        <div class="rates-label">Intro APR</div>
        <div class="rates-value">0%</div>
        <div class="rates-note">First 12 months on purchases &amp; balance transfers</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Standard APR</div>
        <div class="rates-value">16.99–25.99%</div>
        <div class="rates-note">Based on creditworthiness</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Cash Advance APR</div>
        <div class="rates-value">27.99%</div>
        <div class="rates-note">Subject to change</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Late Payment Fee</div>
        <div class="rates-value">$35</div>
        <div class="rates-note">Payment due 25th of each month</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Foreign Transaction</div>
        <div class="rates-value">3%</div>
        <div class="rates-note">Waived for Elite Tier members</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Balance Transfer Fee</div>
        <div class="rates-value">5% / $5</div>
        <div class="rates-note">Whichever is greater</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Min. Credit Score</div>
        <div class="rates-value">650+</div>
        <div class="rates-note">Varies by card type</div>
      </div>
      <div class="rates-item">
        <div class="rates-label">Grace Period</div>
        <div class="rates-value">21 days</div>
        <div class="rates-note">After statement closing date</div>
      </div>
    </div>
  </div>

  <!-- AIRLINE PARTNERS -->
  <div id="airlines" style="margin-top:40px;">
    <div class="section-title">✈️ Airline Partnership Benefits</div>
    <div class="section-subtitle">Exclusive perks with the Travel Rewards Card across our three airline partners.</div>
    <div class="partners-grid">
      <div class="partner-card">
        <div class="partner-name">🔵 Delta Airlines</div>
        <div class="partner-benefit">10% discount on Delta tickets when booked using your rewards balance.</div>
      </div>
      <div class="partner-card">
        <div class="partner-name">🔵 United Airlines</div>
        <div class="partner-benefit">Free checked bag and priority boarding on all United flights.</div>
      </div>
      <div class="partner-card">
        <div class="partner-name">🔵 American Airlines</div>
        <div class="partner-benefit">Earn 25% more miles on every American Airlines purchase.</div>
      </div>
    </div>
  </div>

  <!-- REWARDS TIERS -->
  <div id="tiers" style="margin-top:40px;">
    <div class="section-title">🏅 Rewards Tiers</div>
    <div class="section-subtitle">Your tier is evaluated annually (Jan 1–Dec 31) based on total spending across all Webex Bank cards. Once achieved, status is maintained for the rest of that year and all of the following year.</div>
    <div class="tiers">

      <div class="tier-card tier-basic">
        <div class="tier-header">🎫 Basic Member</div>
        <div class="tier-body">
          <div class="tier-qualify"><strong>Qualification:</strong> All new cardholders start here.</div>
          <ul class="tier-perks">
            <li><span class="perk-icon">💳</span> Standard Cashback: 3% dining, 2% groceries, 1% other</li>
            <li><span class="perk-icon">🔒</span> Zero liability fraud protection</li>
            <li><span class="perk-icon">🌐</span> Full access to online &amp; mobile banking portal</li>
            <li><span class="perk-icon">🔔</span> Real-time transaction alerts</li>
          </ul>
        </div>
      </div>

      <div class="tier-card tier-preferred">
        <div class="tier-header">⭐ Preferred Member</div>
        <div class="tier-body">
          <div class="tier-qualify"><strong>Qualification:</strong> Spend $10,000+ annually.</div>
          <ul class="tier-perks">
            <li><span class="perk-icon">💳</span> Enhanced cashback: 3.5% dining, 2.5% groceries, 1.5% other</li>
            <li><span class="perk-icon">📞</span> Priority customer support line</li>
            <li><span class="perk-icon">🎁</span> Exclusive quarterly offers from partner merchants</li>
            <li><span class="perk-icon">✈️</span> All Basic tier benefits included</li>
          </ul>
        </div>
      </div>

      <div class="tier-card tier-elite">
        <div class="tier-header">💎 Elite Member</div>
        <div class="tier-body">
          <div class="tier-qualify"><strong>Qualification:</strong> Spend $25,000+ annually.</div>
          <ul class="tier-perks">
            <li><span class="perk-icon">💳</span> Maximum cashback: 4% dining, 3% groceries, 2% other</li>
            <li><span class="perk-icon">🌍</span> Foreign transaction fees waived on all cards</li>
            <li><span class="perk-icon">💰</span> Annual $50 statement credit (travel or dining)</li>
            <li><span class="perk-icon">🛎️</span> Dedicated concierge for travel &amp; lifestyle</li>
            <li><span class="perk-icon">🎟️</span> Invitations to exclusive Webex Bank events</li>
            <li><span class="perk-icon">✈️</span> All Preferred tier benefits included</li>
          </ul>
        </div>
      </div>

    </div>
  </div>

  <!-- TERMS & CONDITIONS -->
  <div class="section-block" style="margin-top:40px;">
    <div class="section-block-header">
      <h2>📋 Rewards Program Terms &amp; Conditions</h2>
      <div class="tabs">
        <button class="tab active" onclick="openTab(event,'earning')">Earning</button>
        <button class="tab" onclick="openTab(event,'redemption')">Redemption</button>
        <button class="tab" onclick="openTab(event,'expiration')">Expiration</button>
        <button class="tab" onclick="openTab(event,'account')">Account</button>
        <button class="tab" onclick="openTab(event,'limitations')">Limitations</button>
        <button class="tab" onclick="openTab(event,'security')">Security</button>
        <button class="tab" onclick="openTab(event,'billing')">Billing</button>
      </div>
    </div>

    <div id="earning" class="tab-content active">
      <p>Rewards are earned on most standard purchase transactions, including dining, retail, groceries, and travel. The earning rate depends on your card type, purchase category, and current Rewards Tier.</p>
      <ul>
        <li>Transactions excluded from earning: cash advances, balance transfers, interest charges, fees, and refunded purchases.</li>
        <li>Webex Bank may introduce promotional categories or partner offers with enhanced earning rates.</li>
        <li>Tier-enhanced rates apply automatically once your annual spending threshold is reached.</li>
      </ul>
    </div>

    <div id="redemption" class="tab-content">
      <p>Rewards hold no value until redeemed. Cashback is applied as statement credit, while travel rewards may be redeemed for flights, hotels, or cashback through the Webex Bank Rewards Portal.</p>
      <ul>
        <li>Rewards are personal and cannot be transferred or sold.</li>
        <li>No minimum redemption threshold for Standard Cashback card.</li>
        <li>Travel Rewards points can be applied directly at checkout via partner booking platforms.</li>
        <li>Platinum points redeemable at 1:1 value for travel bookings or statement credits.</li>
      </ul>
    </div>

    <div id="expiration" class="tab-content">
      <p>Rewards validity ranges from two to five years depending on the card type. Specifics are outlined in your cardholder agreement.</p>
      <ul>
        <li>Rewards may be forfeited upon account closure.</li>
        <li>Confirmed fraudulent activity may result in reward forfeiture.</li>
        <li>Unused rewards approaching expiration will trigger email reminders 90 and 30 days in advance.</li>
      </ul>
    </div>

    <div id="account" class="tab-content">
      <p>Accounts must remain active and in good standing to earn and redeem rewards.</p>
      <ul>
        <li>Delinquent or suspended accounts may lose reward privileges during the delinquency period.</li>
        <li>Tier status is evaluated annually; maintaining tier status requires continued qualifying spend.</li>
        <li>Payment due date is the 25th of each month; statements generate on the 1st.</li>
        <li>Customers may request a billing cycle change once every 12 months (account must be in good standing).</li>
      </ul>
    </div>

    <div id="limitations" class="tab-content">
      <p>Rewards cannot be exchanged directly for cash outside of the statement credit redemption mechanism. Returned purchases reverse the earned rewards for that transaction.</p>
      <ul>
        <li>Some benefits may be limited to specific merchants or categories.</li>
        <li>Airline partner discounts are subject to seat availability and partner terms.</li>
        <li>Quarterly partner offers for Preferred and Elite members are subject to change.</li>
      </ul>
    </div>

    <div id="security" class="tab-content">
      <p>Fraudulent or abusive activity disqualifies rewards and may result in forfeiture. Webex Bank reserves the right to modify the program at any time.</p>
      <ul>
        <li>Zero Liability Policy: you are not responsible for unauthorized charges reported within 60 days.</li>
        <li>Cards can be instantly locked via the Webex Bank mobile app or website.</li>
        <li>Fraud disputes are investigated within 7–10 business days. A temporary credit is applied during this period.</li>
        <li>Report fraud: <strong>555-555-5545</strong> or <strong>fraud@webexbank.com</strong></li>
      </ul>
    </div>

    <div id="billing" class="tab-content">
      <p>Webex Bank credit card statements are generated on the 1st of every month. Payments are due on the 25th of each month.</p>
      <ul>
        <li>Grace period: 21 days after the statement closing date.</li>
        <li>Late payments may incur a penalty APR of up to 29.99% in addition to a $35 late fee.</li>
        <li>Balance transfers carry a fee of 5% of the transfer amount or $5, whichever is greater.</li>
        <li>Introductory 0% APR applies to purchases and balance transfers for the first 12 months.</li>
        <li>Digital payments are processed through <strong>NovaPay</strong> — a secure, encrypted payment partner.</li>
      </ul>
    </div>
  </div>

  <!-- FAQ -->
  <div id="faq" style="margin-top:40px;">
    <div class="section-title">❓ Frequently Asked Questions</div>
    <div class="section-subtitle">Quick answers to the most common questions from our cardholders.</div>
    <div class="faq-list">

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">How do I activate my new Webex Bank credit card? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">You can activate your card by calling <strong>555-555-5545</strong>, using the Webex Bank mobile app, or visiting our website's card activation page.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">How do I redeem my rewards? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Log in to your Webex Bank Rewards Portal to redeem cashback as a statement credit, or use travel points for flights and hotels directly through our partner booking platforms.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">Can I change my payment due date? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Yes. You can request a billing cycle change once every 12 months, provided your account is currently in good standing. Contact customer support to initiate this change.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">What should I do if I lose my card? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Immediately lock your card using the Webex Bank mobile app. Then call <strong>555-555-5545</strong> to report the loss and request a replacement. A new card will be issued with a new card number and shipped within 5 business days.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">I see a small charge I don't recognize — should I be concerned? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Yes — even small unknown charges (e.g., $0.99) can indicate fraud, as fraudsters often test cards with small amounts before larger purchases. Contact our Fraud Hotline at <strong>555-555-5545</strong> immediately to report it.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">How do I earn more cashback? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Use your Webex Bank card at Preferred Partner merchants (major grocery chains and fuel stations), reach higher tier status for enhanced rates, and look out for seasonal promotional category offers that provide bonus cashback rates.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">What happens if I miss my payment due date? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">A late fee of $35 may be applied and your interest rate could increase to the default penalty APR (up to 29.99%). We recommend making at least a partial payment to show intent and minimize penalties. Contact us if you're experiencing financial hardship.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">Is the NovaPay payment link sent via email secure? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Yes. Payment links are generated through <strong>NovaPay</strong>, our secure payment partner. Each link is encrypted and unique to your specific transaction session.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question" onclick="toggleFaq(this)">How do I qualify for Elite tier status? <span class="faq-arrow">▼</span></div>
        <div class="faq-answer">Spend $25,000 or more annually across all your Webex Bank credit cards between January 1st and December 31st. Once Elite status is achieved, it is maintained for the remainder of the current year and the entirety of the following year.</div>
      </div>

    </div>
  </div>

  <!-- SUPPORT -->
  <div id="support" style="margin-top:40px;">
    <div class="section-title">📞 Customer Support</div>
    <div class="section-subtitle">Our dedicated team is available to help you with any questions or concerns.</div>
    <div class="contact-grid">
      <div class="contact-card">
        <div class="contact-icon">📞</div>
        <div class="contact-label">Phone</div>
        <div class="contact-value">555-555-5545</div>
        <div class="contact-note">Priority line for Preferred &amp; Elite members</div>
      </div>
      <div class="contact-card">
        <div class="contact-icon">📧</div>
        <div class="contact-label">Email</div>
        <div class="contact-value">support@webexbank.com</div>
        <div class="contact-note">General inquiries</div>
      </div>
      <div class="contact-card">
        <div class="contact-icon">🚨</div>
        <div class="contact-label">Fraud Hotline</div>
        <div class="contact-value">555-555-5545</div>
        <div class="contact-note">fraud@webexbank.com · 24/7</div>
      </div>
      <div class="contact-card">
        <div class="contact-icon">💬</div>
        <div class="contact-label">Live Chat</div>
        <div class="contact-value">Available on website</div>
        <div class="contact-note">Mon–Fri, 8 AM – 8 PM EST</div>
      </div>
      <div class="contact-card">
        <div class="contact-icon">📍</div>
        <div class="contact-label">Mailing Address</div>
        <div class="contact-value">123 Finance Avenue</div>
        <div class="contact-note">Faketown, FS 12345</div>
      </div>
    </div>
  </div>

</div><!-- /container -->

<div class="footer">
  © 2026 Webex Bank — All rights reserved. Rates and terms subject to change. This is a simulated demo environment.
</div>

<script>
function openTab(evt, tabName) {
  document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
  document.getElementById(tabName).classList.add('active');
  evt.currentTarget.classList.add('active');
}

function toggleFaq(el) {
  const item = el.parentElement;
  item.classList.toggle('open');
}
</script>

</body>
</html>
