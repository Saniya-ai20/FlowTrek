<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="theme-color" content="#12351E">
  <link rel="manifest" href="/manifest.json">
  <title>FlowTrek - Mobile & Desktop Expense Tracker</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@500;600;700&family=Inter:wght@400;500;600;700&family=Outfit:wght@500;600;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --bg: #F4F9F5;
      --surface: #FFFFFF;
      --surface-2: #EAF5ED;
      --primary: #27964A;
      --primary-dark: #12351E;
      --primary-light: #C9ECCF;
      --danger: #EF4444;
      --warning: #F59E0B;
      --text: #172B1D;
      --text-muted: #526B59;
      --border: #D8EADF;
      --card-shadow: 0 4px 14px rgba(18, 53, 30, 0.05);
    }

    [data-theme="dark"] {
      --bg: #0C1510;
      --surface: #132219;
      --surface-2: #1A2F22;
      --primary: #34D399;
      --primary-dark: #064E3B;
      --primary-light: #065F46;
      --text: #ECFDF5;
      --text-muted: #A7F3D0;
      --border: #213A2B;
      --card-shadow: 0 4px 14px rgba(0, 0, 0, 0.4);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
    body { font-family: 'Inter', system-ui, -apple-system, sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; transition: background 0.2s; }
    h1, h2, h3 { font-family: 'Fraunces', Georgia, serif; }

    .layout { display: flex; min-height: 100vh; }
    .sidebar {
      width: 250px; background: var(--primary-dark); color: white; padding: 28px 18px;
      display: flex; flex-direction: column; gap: 6px; flex-shrink: 0;
    }
    .brand { display: flex; align-items: center; gap: 10px; margin-bottom: 28px; color: white; }
    .brand-icon { width: 38px; height: 38px; background: var(--primary); border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 20px; color: white; }
    .brand-title { font-size: 22px; font-weight: 700; font-family: 'Fraunces', serif; }
    
    .nav-btn {
      display: flex; align-items: center; gap: 12px; padding: 12px 14px; border-radius: 12px;
      color: #C9ECCF; background: none; border: none; font-size: 14.5px; font-weight: 500;
      cursor: pointer; text-align: left; transition: all 0.15s ease;
    }
    .nav-btn:hover, .nav-btn.active { background: rgba(255,255,255,0.1); color: white; }
    .nav-btn.active { background: var(--primary); color: white; font-weight: 600; }

    .main { flex: 1; padding: 32px 40px; min-width: 0; padding-bottom: 80px; }
    .header-row { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 24px; flex-wrap: wrap; gap: 16px; }
    .page-title { font-size: 28px; color: var(--primary-dark); margin-bottom: 4px; }
    [data-theme="dark"] .page-title { color: var(--primary); }
    .page-sub { color: var(--text-muted); font-size: 14px; }

    .btn {
      background: var(--primary); color: white; border: none; padding: 12px 22px; border-radius: 99px;
      font-weight: 600; cursor: pointer; display: inline-flex; align-items: center; gap: 8px; font-size: 14px;
      box-shadow: 0 4px 14px rgba(39,150,74,0.3); transition: transform 0.15s; min-height: 44px;
    }
    .btn:active { transform: scale(0.96); }
    .btn-outline { background: var(--surface); color: var(--text); border: 1.5px solid var(--border); box-shadow: none; }
    .btn-outline:hover { background: var(--surface-2); }
    .btn-danger { background: #FEF2F2; color: var(--danger); border: 1px solid #FCA5A5; box-shadow: none; }

    .banner {
      background: var(--surface); border: 1px solid var(--border); border-radius: 16px; padding: 20px;
      margin-bottom: 24px; box-shadow: var(--card-shadow);
    }
    .progress-bar-bg { width: 100%; height: 10px; background: var(--border); border-radius: 99px; overflow: hidden; margin-top: 8px; }
    .progress-bar-fill { height: 100%; background: var(--primary); border-radius: 99px; transition: width 0.4s ease; }

    .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 14px; margin-bottom: 24px; }
    .stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: 16px; padding: 18px; box-shadow: var(--card-shadow); }
    .stat-label { font-size: 11.5px; color: var(--text-muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; }
    .stat-val { font-size: 22px; font-weight: 700; color: var(--primary-dark); margin-top: 4px; font-family: 'Fraunces', serif; word-break: break-word; }
    [data-theme="dark"] .stat-val { color: var(--primary); }

    .charts-grid { display: grid; grid-template-columns: 1.1fr 1fr; gap: 20px; margin-bottom: 24px; }
    .panel { background: var(--surface); border: 1px solid var(--border); border-radius: 18px; padding: 20px; box-shadow: var(--card-shadow); overflow-x: auto; }
    .panel h3 { font-size: 16px; color: var(--primary-dark); margin-bottom: 16px; }
    [data-theme="dark"] .panel h3 { color: var(--primary); }

    .toolbar { display: flex; gap: 10px; flex-wrap: wrap; margin-bottom: 20px; }
    .search-inp, .filter-select {
      padding: 12px 14px; border-radius: 10px; border: 1.5px solid var(--border);
      font-size: 16px; background: var(--surface); color: var(--text); outline: none; min-height: 44px;
    }
    .search-inp { flex: 1; min-width: 180px; }

    .table-wrap { overflow-x: auto; -webkit-overflow-scrolling: touch; }
    .table { width: 100%; border-collapse: collapse; margin-top: 6px; min-width: 500px; }
    .table th, .table td { padding: 12px 10px; text-align: left; border-bottom: 1px solid var(--border); font-size: 13.5px; }
    .table th { color: var(--text-muted); font-weight: 600; font-size: 11.5px; text-transform: uppercase; }

    .badge { padding: 4px 10px; border-radius: 99px; font-size: 11.5px; font-weight: 600; color: white; display: inline-block; }
    .badge-food { background: #27964A; }
    .badge-travel { background: #3B82F6; }
    .badge-shopping { background: #EC4899; }
    .badge-bills { background: #F59E0B; }

    /* Mobile Bottom Navigation Bar */
    .mobile-nav {
      display: none; position: fixed; bottom: 0; left: 0; right: 0; z-index: 999;
      background: var(--primary-dark); border-top: 1px solid rgba(255,255,255,0.1);
      padding: 8px 12px; justify-content: space-around; backdrop-filter: blur(10px);
    }
    .mobile-tab {
      display: flex; flex-direction: column; align-items: center; gap: 4px;
      color: #C9ECCF; font-size: 11px; font-weight: 600; background: none; border: none; padding: 6px 12px; border-radius: 10px;
    }
    .mobile-tab.active { color: white; background: var(--primary); }

    .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); backdrop-filter: blur(4px); display: none; align-items: center; justify-content: center; z-index: 1000; padding: 16px; }
    .modal { background: var(--surface); border-radius: 20px; padding: 24px; width: 100%; max-width: 480px; box-shadow: 0 20px 50px rgba(0,0,0,0.3); border: 1px solid var(--border); max-height: 90vh; overflow-y: auto; }
    .form-group { margin-bottom: 14px; }
    .form-group label { display: block; font-size: 13px; font-weight: 600; margin-bottom: 6px; color: var(--primary-dark); }
    [data-theme="dark"] .form-group label { color: var(--primary); }
    .form-group input, .form-group select, .form-group textarea {
      width: 100%; padding: 12px 14px; border-radius: 10px; border: 1.5px solid var(--border);
      font-size: 16px; background: var(--bg); color: var(--text); outline: none;
    }

    @media (max-width: 768px) {
      .layout { flex-direction: column; }
      .sidebar { display: none; }
      .mobile-nav { display: flex; }
      .charts-grid { grid-template-columns: 1fr; }
      .main { padding: 20px 16px 90px 16px; }
      .page-title { font-size: 24px; }
    }
  </style>
</head>
<body>
  <div class="layout">
    <!-- Desktop Sidebar -->
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-icon">🌿</div>
        <span class="brand-title">FlowTrek</span>
      </div>
      <button class="nav-btn active d-nav" onclick="switchTab('overview')">📊 Dashboard</button>
      <button class="nav-btn d-nav" onclick="openAddModal()">➕ Add Expense</button>
      <button class="nav-btn d-nav" onclick="switchTab('expenses')">📜 All Expenses</button>
      <button class="nav-btn d-nav" onclick="switchTab('report')">📑 Monthly Report</button>

      <div style="margin-top: auto; padding-top: 16px; border-top: 1px solid rgba(255,255,255,0.12); display: flex; flex-direction: column; gap: 8px;">
        <button class="nav-btn" style="background: rgba(255,255,255,0.06);" onclick="toggleDark()">
          <span id="theme-icon">🌙</span> <span id="theme-label">Dark Mode</span>
        </button>
        <button class="nav-btn" style="background: rgba(255,255,255,0.06); color: #F87171;" onclick="resetDemo()">
          🔄 Reset Demo Data
        </button>
        <div style="font-size: 12px; color: #C9ECCF; padding: 6px 12px;">
          Signed in as <strong style="color: white;">Priya Sharma</strong>
        </div>
      </div>
    </aside>

    <main class="main">
      <!-- Top Mobile Brand Header -->
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 12px; border-bottom: 1px solid var(--border);" class="mobile-only">
        <div style="display: flex; align-items: center; gap: 8px;">
          <div style="width: 32px; height: 32px; background: var(--primary); color: white; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 18px;">🌿</div>
          <span style="font-size: 20px; font-weight: 700; font-family: 'Fraunces', serif; color: var(--primary-dark);">FlowTrek</span>
        </div>
        <button onclick="toggleDark()" style="background: var(--surface); border: 1px solid var(--border); border-radius: 99px; padding: 6px 12px; font-size: 13px; font-weight: 600;">
          <span id="m-theme-icon">🌙</span>
        </button>
      </div>

      <!-- Dashboard Overview -->
      <div id="overview-tab">
        <div class="header-row">
          <div>
            <h2 class="page-title">Hi, Priya 👋</h2>
            <div class="page-sub">Here is your live expense summary for July 2026.</div>
          </div>
          <button class="btn" onclick="openAddModal()">+ Log Expense</button>
        </div>

        <div class="banner">
          <div style="display: flex; justify-content: space-between; font-weight: 600; font-size: 13.5px;">
            <span>🎯 Monthly Budget Target (₹55,000.00)</span>
            <span id="budget-text">₹23,220.00 / ₹55,000.00 (42%)</span>
          </div>
          <div class="progress-bar-bg">
            <div class="progress-bar-fill" id="budget-bar" style="width: 42%;"></div>
          </div>
        </div>

        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-label">Spent This Month</div>
            <div class="stat-val" id="stat-month">₹23,220.00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">All Time Total</div>
            <div class="stat-val" id="stat-all">₹79,720.00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">Total Transactions</div>
            <div class="stat-val" id="stat-count">8</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">Top Category</div>
            <div class="stat-val" id="stat-top">Travel</div>
          </div>
        </div>

        <div class="charts-grid">
          <div class="panel">
            <h3>Category Breakdown (This Month)</h3>
            <div style="position: relative; height: 230px;">
              <canvas id="pieChart"></canvas>
            </div>
          </div>
          <div class="panel">
            <h3>Historical 6-Month Trend</h3>
            <div style="position: relative; height: 230px;">
              <canvas id="barChart"></canvas>
            </div>
          </div>
        </div>
      </div>

      <!-- Expenses List Tab -->
      <div id="expenses-tab" style="display: none;">
        <div class="header-row">
          <div>
            <h2 class="page-title">All Transactions</h2>
            <div class="page-sub">Filter, inspect receipts, or export expenses.</div>
          </div>
          <button class="btn btn-outline" onclick="exportCSV()">📥 Export CSV</button>
        </div>

        <div class="toolbar">
          <input class="search-inp" id="search-inp" placeholder="Search description..." oninput="renderStats()">
          <select class="filter-select" id="filter-cat" onchange="renderStats()">
            <option value="all">All Categories</option>
            <option value="food">Food & Dining</option>
            <option value="travel">Travel & Transit</option>
            <option value="shopping">Shopping & Retail</option>
            <option value="bills">Bills & Utilities</option>
          </select>
          <select class="filter-select" id="filter-method" onchange="renderStats()">
            <option value="all">All Methods</option>
            <option value="UPI">UPI</option>
            <option value="Paytm">Paytm</option>
            <option value="PhonePe">PhonePe</option>
            <option value="PayPal">PayPal</option>
          </select>
        </div>

        <div class="panel">
          <div class="table-wrap">
            <table class="table">
              <thead>
                <tr>
                  <th>Date</th>
                  <th>Description</th>
                  <th>Category</th>
                  <th>Method</th>
                  <th>Amount</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody id="expenses-tbody"></tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- Monthly Report Tab -->
      <div id="report-tab" style="display: none;">
        <div class="header-row">
          <div>
            <h2 class="page-title">Monthly Statement</h2>
            <div class="page-sub">Clean formatted table ready for PDF printing.</div>
          </div>
          <button class="btn" onclick="window.print()">🖨️ Print / Save PDF</button>
        </div>

        <div class="panel">
          <h3 id="report-title">July 2026 Statement</h3>
          <div class="table-wrap">
            <table class="table">
              <thead>
                <tr><th>Category</th><th>Transactions</th><th>Total Amount</th><th>% Share</th></tr>
              </thead>
              <tbody id="report-tbody"></tbody>
            </table>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- Mobile Bottom Tab Navigation -->
  <nav class="mobile-nav">
    <button class="mobile-tab active m-tab" onclick="switchTab('overview')">
      <span style="font-size: 18px;">📊</span>
      <span>Dashboard</span>
    </button>
    <button class="mobile-tab m-tab" onclick="openAddModal()">
      <span style="font-size: 18px;">➕</span>
      <span>Add</span>
    </button>
    <button class="mobile-tab m-tab" onclick="switchTab('expenses')">
      <span style="font-size: 18px;">📜</span>
      <span>Expenses</span>
    </button>
    <button class="mobile-tab m-tab" onclick="switchTab('report')">
      <span style="font-size: 18px;">📑</span>
      <span>Report</span>
    </button>
  </nav>

  <!-- Add Modal -->
  <div class="modal-overlay" id="modal">
    <div class="modal">
      <h3 style="margin-bottom: 16px;" id="modal-title">Add New Expense</h3>
      <form onsubmit="saveExpense(event)">
        <div class="form-group">
          <label>Description / Merchant</label>
          <input required id="inp-desc" placeholder="e.g. Swiggy Lunch / Weekly Groceries">
        </div>
        <div class="form-group">
          <label>Amount (₹)</label>
          <input required type="number" step="0.01" id="inp-amount" placeholder="0.00">
        </div>
        <div class="form-group">
          <label>Category</label>
          <select id="inp-cat">
            <option value="food">Food & Dining</option>
            <option value="travel">Travel & Transit</option>
            <option value="shopping">Shopping & Retail</option>
            <option value="bills">Bills & Utilities</option>
          </select>
        </div>
        <div class="form-group">
          <label>Payment Method</label>
          <select id="inp-method">
            <option value="UPI">UPI</option>
            <option value="Paytm">Paytm</option>
            <option value="PhonePe">PhonePe</option>
            <option value="PayPal">PayPal</option>
          </select>
        </div>
        <div class="form-group">
          <label>Date</label>
          <input type="date" id="inp-date" required>
        </div>
        <div style="display: flex; justify-content: flex-end; gap: 10px; margin-top: 24px;">
          <button type="button" class="btn btn-outline" onclick="closeAddModal()">Cancel</button>
          <button type="submit" class="btn">Save Expense</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Receipt Modal -->
  <div class="modal-overlay" id="receipt-modal">
    <div class="modal" style="max-width: 380px; text-align: center;">
      <h3 style="margin-bottom: 6px; font-family: 'Fraunces', serif;">Digital Receipt</h3>
      <div style="font-size: 12px; color: var(--text-muted); margin-bottom: 20px;" id="rcpt-id">Ref #exp_101</div>

      <div style="text-align: left; display: flex; flex-direction: column; gap: 12px; font-size: 14px; border-top: 1px dashed var(--border); border-bottom: 1px dashed var(--border); padding: 16px 0;">
        <div style="display: flex; justify-content: space-between;"><span style="color: var(--text-muted)">Item</span><strong id="rcpt-desc">Zara</strong></div>
        <div style="display: flex; justify-content: space-between;"><span style="color: var(--text-muted)">Category</span><span id="rcpt-cat">Shopping</span></div>
        <div style="display: flex; justify-content: space-between;"><span style="color: var(--text-muted)">Date</span><span id="rcpt-date">2026-07-15</span></div>
        <div style="display: flex; justify-content: space-between;"><span style="color: var(--text-muted)">Method</span><span id="rcpt-method">UPI</span></div>
        <div style="display: flex; justify-content: space-between; font-size: 18px; border-top: 2px solid var(--border); padding-top: 10px;"><strong>Total Paid</strong><strong id="rcpt-amount" style="color: var(--primary)">₹4,500.00</strong></div>
      </div>

      <div style="margin-top: 20px;">
        <button class="btn btn-outline" style="width: 100%; justify-content: center;" onclick="closeReceiptModal()">Close Receipt</button>
      </div>
    </div>
  </div>

  <script>
    const INITIAL_EXPENSES = [
      { id: 'exp_1', amount: 1450, category: 'food', description: 'Weekly Grocery at Nature Basket', date: '2026-07-20', method: 'UPI' },
      { id: 'exp_2', amount: 3200, category: 'bills', description: 'Electricity & High-Speed Internet', date: '2026-07-18', method: 'Paytm' },
      { id: 'exp_3', amount: 4500, category: 'shopping', description: 'Zara Summer Wear & Shoes', date: '2026-07-15', method: 'PhonePe' },
      { id: 'exp_4', amount: 12500, category: 'travel', description: 'Goa Flight Tickets (IndiGo)', date: '2026-07-10', method: 'PayPal' },
      { id: 'exp_5', amount: 1570, category: 'food', description: 'Swiggy Gourmet Team Lunch', date: '2026-07-05', method: 'UPI' },
      { id: 'exp_6', amount: 5400, category: 'travel', description: 'Car Fuel & Annual Servicing', date: '2026-06-22', method: 'UPI' },
      { id: 'exp_7', amount: 8500, category: 'shopping', description: 'Ergonomic Desk Chair', date: '2026-05-18', method: 'UPI' },
      { id: 'exp_8', amount: 14200, category: 'travel', description: 'Coorg Homestay Staycation', date: '2026-04-12', method: 'PayPal' }
    ];

    let expenses = JSON.parse(localStorage.getItem('flowtrek_standalone_exp')) || INITIAL_EXPENSES;
    let isDark = localStorage.getItem('flowtrek_theme') === 'dark';
    let pieChart, barChart;

    if (isDark) document.documentElement.setAttribute('data-theme', 'dark');

    function toggleDark() {
      isDark = !isDark;
      if (isDark) {
        document.documentElement.setAttribute('data-theme', 'dark');
        localStorage.setItem('flowtrek_theme', 'dark');
        document.getElementById('theme-icon').innerText = '☀️';
        document.getElementById('m-theme-icon').innerText = '☀️';
        document.getElementById('theme-label').innerText = 'Light Mode';
      } else {
        document.documentElement.removeAttribute('data-theme');
        localStorage.setItem('flowtrek_theme', 'light');
        document.getElementById('theme-icon').innerText = '🌙';
        document.getElementById('m-theme-icon').innerText = '🌙';
        document.getElementById('theme-label').innerText = 'Dark Mode';
      }
      renderStats();
    }

    function resetDemo() {
      expenses = [...INITIAL_EXPENSES];
      localStorage.setItem('flowtrek_standalone_exp', JSON.stringify(expenses));
      renderStats();
      alert('Demo data reset successfully!');
    }

    function saveStorage() {
      localStorage.setItem('flowtrek_standalone_exp', JSON.stringify(expenses));
    }

    function fmt(num) {
      return '₹' + Number(num).toLocaleString('en-IN', { minimumFractionDigits: 2 });
    }

    function renderStats() {
      const nowMonth = '2026-07';
      const thisMonthExp = expenses.filter(e => e.date.startsWith(nowMonth));
      const totalThisMonth = thisMonthExp.reduce((s, e) => s + Number(e.amount), 0);
      const totalAll = expenses.reduce((s, e) => s + Number(e.amount), 0);
      const budgetLimit = 55000;
      const pct = Math.min(100, Math.round((totalThisMonth / budgetLimit) * 100));

      document.getElementById('stat-month').innerText = fmt(totalThisMonth);
      document.getElementById('stat-all').innerText = fmt(totalAll);
      document.getElementById('stat-count').innerText = expenses.length;
      document.getElementById('budget-text').innerText = `${fmt(totalThisMonth)} / ${fmt(budgetLimit)} (${pct}%)`;
      document.getElementById('budget-bar').style.width = pct + '%';

      // Category map
      const cats = { food: 0, travel: 0, shopping: 0, bills: 0 };
      thisMonthExp.forEach(e => { cats[e.category] = (cats[e.category] || 0) + Number(e.amount); });

      const topCat = Object.entries(cats).sort((a,b) => b[1]-a[1])[0];
      document.getElementById('stat-top').innerText = topCat && topCat[1] > 0 ? topCat[0].toUpperCase() : '—';

      // Pie chart render
      const ctxPie = document.getElementById('pieChart').getContext('2d');
      if (pieChart) pieChart.destroy();
      pieChart = new Chart(ctxPie, {
        type: 'doughnut',
        data: {
          labels: ['Food', 'Travel', 'Shopping', 'Bills'],
          datasets: [{
            data: [cats.food, cats.travel, cats.shopping, cats.bills],
            backgroundColor: ['#27964A', '#3B82F6', '#EC4899', '#F59E0B']
          }]
        },
        options: { responsive: true, maintainAspectRatio: false }
      });

      // Bar chart render
      const ctxBar = document.getElementById('barChart').getContext('2d');
      if (barChart) barChart.destroy();
      barChart = new Chart(ctxBar, {
        type: 'bar',
        data: {
          labels: ['Apr', 'May', 'Jun', 'Jul'],
          datasets: [{
            label: 'Monthly Spend',
            data: [14200, 8500, 5400, totalThisMonth],
            backgroundColor: '#27964A',
            borderRadius: 8
          }]
        },
        options: { responsive: true, maintainAspectRatio: false }
      });

      // Filtered expenses
      const search = (document.getElementById('search-inp')?.value || '').toLowerCase();
      const fCat = document.getElementById('filter-cat')?.value || 'all';
      const fMethod = document.getElementById('filter-method')?.value || 'all';

      const filtered = expenses.filter(e => {
        const matchSearch = !search || e.description.toLowerCase().includes(search);
        const matchCat = fCat === 'all' || e.category === fCat;
        const matchMethod = fMethod === 'all' || e.method === fMethod;
        return matchSearch && matchCat && matchMethod;
      });

      // Render Expenses Table
      const tbody = document.getElementById('expenses-tbody');
      tbody.innerHTML = filtered.map(e => `
        <tr>
          <td>${e.date}</td>
          <td><strong>${e.description}</strong></td>
          <td><span class="badge badge-${e.category}">${e.category.toUpperCase()}</span></td>
          <td>${e.method}</td>
          <td><strong>${fmt(e.amount)}</strong></td>
          <td>
            <button onclick="viewReceipt('${e.id}')" style="color:var(--primary); border:none; background:none; cursor:pointer; font-weight:600; margin-right:8px;">Receipt</button>
            <button onclick="deleteExp('${e.id}')" style="color:var(--danger); border:none; background:none; cursor:pointer;">Delete</button>
          </td>
        </tr>
      `).join('');

      // Render Report
      const rbody = document.getElementById('report-tbody');
      rbody.innerHTML = Object.entries(cats).map(([k, v]) => `
        <tr>
          <td style="text-transform:capitalize;">${k}</td>
          <td>${thisMonthExp.filter(e => e.category === k).length}</td>
          <td><strong>${fmt(v)}</strong></td>
          <td>${totalThisMonth > 0 ? Math.round((v / totalThisMonth) * 100) : 0}%</td>
        </tr>
      `).join('');
    }

    function switchTab(tab) {
      document.getElementById('overview-tab').style.display = tab === 'overview' ? 'block' : 'none';
      document.getElementById('expenses-tab').style.display = tab === 'expenses' ? 'block' : 'none';
      document.getElementById('report-tab').style.display = tab === 'report' ? 'block' : 'none';
      
      document.querySelectorAll('.d-nav').forEach((b, i) => {
        b.classList.toggle('active', (i === 0 && tab === 'overview') || (i === 2 && tab === 'expenses') || (i === 3 && tab === 'report'));
      });
      document.querySelectorAll('.m-tab').forEach((b, i) => {
        b.classList.toggle('active', (i === 0 && tab === 'overview') || (i === 2 && tab === 'expenses') || (i === 3 && tab === 'report'));
      });
    }

    function openAddModal() {
      document.getElementById('inp-date').value = new Date().toISOString().slice(0,10);
      document.getElementById('modal').style.display = 'flex';
    }
    function closeAddModal() { document.getElementById('modal').style.display = 'none'; }

    function viewReceipt(id) {
      const e = expenses.find(x => x.id === id);
      if (!e) return;
      document.getElementById('rcpt-id').innerText = 'Ref #' + e.id;
      document.getElementById('rcpt-desc').innerText = e.description;
      document.getElementById('rcpt-cat').innerText = e.category.toUpperCase();
      document.getElementById('rcpt-date').innerText = e.date;
      document.getElementById('rcpt-method').innerText = e.method;
      document.getElementById('rcpt-amount').innerText = fmt(e.amount);
      document.getElementById('receipt-modal').style.display = 'flex';
    }

    function closeReceiptModal() {
      document.getElementById('receipt-modal').style.display = 'none';
    }

    function saveExpense(e) {
      e.preventDefault();
      const newE = {
        id: 'exp_' + Math.random().toString(36).slice(2,7),
        description: document.getElementById('inp-desc').value,
        amount: Number(document.getElementById('inp-amount').value),
        category: document.getElementById('inp-cat').value,
        method: document.getElementById('inp-method').value,
        date: document.getElementById('inp-date').value
      };
      expenses.unshift(newE);
      saveStorage();
      closeAddModal();
      renderStats();
    }

    function deleteExp(id) {
      expenses = expenses.filter(e => e.id !== id);
      saveStorage();
      renderStats();
    }

    function exportCSV() {
      const rows = [["Date", "Description", "Category", "Method", "Amount"], ...expenses.map(e => [e.date, e.description, e.category, e.method, e.amount])];
      const csv = rows.map(r => r.join(",")).join("\n");
      const blob = new Blob([csv], { type: "text/csv" });
      const a = document.createElement("a");
      a.href = URL.createObjectURL(blob);
      a.download = "FlowTrek_Expenses.csv";
      a.click();
    }

    renderStats();
  </script>
</body>
</html>
