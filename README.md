<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Baba - Advanced Trading Analysis</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #1a1a2e;
            --secondary: #16213e;
            --accent: #0f3460;
            --highlight: #e94560;
            --text: #ffffff;
            --text-secondary: #b0b0b0;
            --success: #00b894;
            --warning: #fdcb6e;
            --danger: #e84393;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: var(--primary);
            color: var(--text);
            overflow-x: hidden;
        }
        
        .container {
            display: grid;
            grid-template-columns: 250px 1fr;
            grid-template-rows: 70px 1fr;
            grid-template-areas: 
                "sidebar header"
                "sidebar main";
            min-height: 100vh;
        }
        
        /* Header Styles */
        header {
            grid-area: header;
            background: var(--secondary);
            padding: 0 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo h1 {
            font-size: 1.8rem;
            background: linear-gradient(90deg, var(--highlight), var(--warning));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        
        .search-bar {
            display: flex;
            align-items: center;
            background: rgba(255,255,255,0.1);
            border-radius: 20px;
            padding: 8px 15px;
            width: 300px;
        }
        
        .search-bar input {
            background: transparent;
            border: none;
            color: var(--text);
            width: 100%;
            margin-left: 10px;
            outline: none;
        }
        
        .user-actions {
            display: flex;
            gap: 15px;
        }
        
        .btn {
            padding: 8px 15px;
            border-radius: 5px;
            border: none;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .btn-primary {
            background: var(--highlight);
            color: white;
        }
        
        .btn-outline {
            background: transparent;
            border: 1px solid var(--highlight);
            color: var(--highlight);
        }
        
        /* Sidebar Styles */
        .sidebar {
            grid-area: sidebar;
            background: var(--secondary);
            padding: 20px 0;
            border-right: 1px solid rgba(255,255,255,0.1);
        }
        
        .nav-item {
            padding: 12px 20px;
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .nav-item:hover, .nav-item.active {
            background: var(--accent);
            border-left: 4px solid var(--highlight);
        }
        
        .nav-section {
            margin-top: 20px;
            padding: 0 20px 10px;
            color: var(--text-secondary);
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        /* Main Content Styles */
        main {
            grid-area: main;
            padding: 20px;
            overflow-y: auto;
        }
        
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .card {
            background: var(--secondary);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        .card-header {
            display: flex;
            justify-content: between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .card-title {
            font-size: 1.2rem;
            font-weight: 600;
        }
        
        .price-up {
            color: var(--success);
        }
        
        .price-down {
            color: var(--danger);
        }
        
        .chart-container {
            height: 250px;
            margin-top: 10px;
        }
        
        .signal-badge {
            display: inline-block;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
        }
        
        .signal-buy {
            background: rgba(0, 184, 148, 0.2);
            color: var(--success);
        }
        
        .signal-sell {
            background: rgba(232, 67, 147, 0.2);
            color: var(--danger);
        }
        
        .signal-hold {
            background: rgba(253, 203, 110, 0.2);
            color: var(--warning);
        }
        
        .news-feed {
            max-height: 400px;
            overflow-y: auto;
        }
        
        .news-item {
            padding: 15px 0;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .news-item:last-child {
            border-bottom: none;
        }
        
        .news-source {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
            font-size: 0.8rem;
            color: var(--text-secondary);
        }
        
        .news-content {
            font-size: 0.9rem;
            line-height: 1.4;
        }
        
        .news-highlight {
            color: var(--highlight);
            font-weight: 600;
        }
        
        .indicators-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            margin-top: 15px;
        }
        
        .indicator {
            text-align: center;
            padding: 10px;
            background: rgba(255,255,255,0.05);
            border-radius: 8px;
        }
        
        .indicator-value {
            font-size: 1.2rem;
            font-weight: 600;
            margin: 5px 0;
        }
        
        .indicator-label {
            font-size: 0.8rem;
            color: var(--text-secondary);
        }
        
        .full-width {
            grid-column: 1 / -1;
        }
        
        .trades-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        
        .trades-table th, .trades-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .trades-table th {
            color: var(--text-secondary);
            font-weight: 500;
            font-size: 0.9rem;
        }
        
        .progress-bar {
            height: 6px;
            background: rgba(255,255,255,0.1);
            border-radius: 3px;
            overflow: hidden;
            margin-top: 5px;
        }
        
        .progress-fill {
            height: 100%;
            border-radius: 3px;
        }
        
        .progress-high {
            background: var(--success);
            width: 85%;
        }
        
        .progress-medium {
            background: var(--warning);
            width: 65%;
        }
        
        .progress-low {
            background: var(--danger);
            width: 40%;
        }
        
        .liquidity-pool {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .pool-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .pool-chart {
            width: 80px;
            height: 40px;
        }
        
        .institution-indicator {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .institution-bar {
            flex-grow: 1;
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 4px;
            overflow: hidden;
            position: relative;
        }
        
        .institution-fill {
            height: 100%;
            border-radius: 4px;
            background: var(--accent);
        }
        
        .tooltip {
            position: relative;
            display: inline-block;
        }
        
        .tooltip .tooltiptext {
            visibility: hidden;
            width: 200px;
            background-color: var(--accent);
            color: var(--text);
            text-align: center;
            border-radius: 6px;
            padding: 10px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -100px;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 0.8rem;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }
        
        .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
        
        @media (max-width: 1200px) {
            .dashboard-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .full-width {
                grid-column: 1 / -1;
            }
        }
        
        @media (max-width: 768px) {
            .container {
                grid-template-columns: 1fr;
                grid-template-areas: 
                    "header"
                    "main";
            }
            
            .sidebar {
                display: none;
            }
            
            .dashboard-grid {
                grid-template-columns: 1fr;
            }
            
            .indicators-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <i class="fas fa-chart-line"></i>
                <h1>Baba Trading</h1>
            </div>
            
            <div class="search-bar">
                <i class="fas fa-search"></i>
                <input type="text" placeholder="Search assets, news, indicators...">
            </div>
            
            <div class="user-actions">
                <button class="btn btn-outline"><i class="fas fa-bell"></i></button>
                <button class="btn btn-primary">Connect Exchange</button>
            </div>
        </header>
        
        <aside class="sidebar">
            <div class="nav-item active">
                <i class="fas fa-home"></i>
                <span>Dashboard</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-chart-bar"></i>
                <span>Market Analysis</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-coins"></i>
                <span>Crypto Assets</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-chart-pie"></i>
                <span>Portfolio</span>
            </div>
            
            <div class="nav-section">Analysis Tools</div>
            
            <div class="nav-item">
                <i class="fas fa-bolt"></i>
                <span>Real-time Signals</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-newspaper"></i>
                <span>News Sentiment</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-water"></i>
                <span>Liquidity Pools</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-building"></i>
                <span>Institution Flow</span>
            </div>
            
            <div class="nav-section">Settings</div>
            
            <div class="nav-item">
                <i class="fas fa-cog"></i>
                <span>Preferences</span>
            </div>
            
            <div class="nav-item">
                <i class="fas fa-robot"></i>
                <span>AI Configuration</span>
            </div>
        </aside>
        
        <main>
            <div class="dashboard-grid">
                <!-- Bitcoin Card -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fab fa-bitcoin"></i> Bitcoin (BTC)
                        </div>
                        <span class="signal-badge signal-buy">BUY</span>
                    </div>
                    <div class="price-up">$43,250.75 <span>+2.4%</span></div>
                    <div class="chart-container">
                        <canvas id="btcChart"></canvas>
                    </div>
                    <div class="indicators-grid">
                        <div class="indicator">
                            <div class="indicator-label">RSI</div>
                            <div class="indicator-value">54.2</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">MACD</div>
                            <div class="indicator-value">+124.5</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Volume</div>
                            <div class="indicator-value">28.5B</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Support</div>
                            <div class="indicator-value">$41,200</div>
                        </div>
                    </div>
                </div>
                
                <!-- Ethereum Card -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fab fa-ethereum"></i> Ethereum (ETH)
                        </div>
                        <span class="signal-badge signal-hold">HOLD</span>
                    </div>
                    <div class="price-up">$2,380.45 <span>+1.2%</span></div>
                    <div class="chart-container">
                        <canvas id="ethChart"></canvas>
                    </div>
                    <div class="indicators-grid">
                        <div class="indicator">
                            <div class="indicator-label">RSI</div>
                            <div class="indicator-value">48.7</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">MACD</div>
                            <div class="indicator-value">-32.1</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Volume</div>
                            <div class="indicator-value">14.2B</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Resistance</div>
                            <div class="indicator-value">$2,450</div>
                        </div>
                    </div>
                </div>
                
                <!-- Gold Card -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fas fa-gem"></i> Gold (XAU)
                        </div>
                        <span class="signal-badge signal-buy">BUY</span>
                    </div>
                    <div class="price-up">$1,985.30 <span>+0.8%</span></div>
                    <div class="chart-container">
                        <canvas id="goldChart"></canvas>
                    </div>
                    <div class="indicators-grid">
                        <div class="indicator">
                            <div class="indicator-label">RSI</div>
                            <div class="indicator-value">62.3</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">MACD</div>
                            <div class="indicator-value">+18.7</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Volume</div>
                            <div class="indicator-value">182.5M</div>
                        </div>
                        <div class="indicator">
                            <div class="indicator-label">Support</div>
                            <div class="indicator-value">$1,950</div>
                        </div>
                    </div>
                </div>
                
                <!-- News Feed Card -->
                <div class="card full-width">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fas fa-newspaper"></i> News & Sentiment Analysis
                        </div>
                    </div>
                    <div class="news-feed">
                        <div class="news-item">
                            <div class="news-source">
                                <i class="fas fa-user-tie"></i>
                                <span>Fed Chair Powell • 2 hours ago</span>
                            </div>
                            <div class="news-content">
                                "Inflation remains elevated, but we're seeing positive signs. We will continue to monitor data closely before making any policy adjustments."
                                <span class="news-highlight">[Market Impact: Neutral]</span>
                            </div>
                        </div>
                        
                        <div class="news-item">
                            <div class="news-source">
                                <i class="fab fa-twitter"></i>
                                <span>Donald Trump • 4 hours ago</span>
                            </div>
                            <div class="news-content">
                                "The economy is doing terribly under this administration. Our great American companies are suffering from bad policies!"
                                <span class="news-highlight">[Market Impact: Slightly Negative]</span>
                            </div>
                        </div>
                        
                        <div class="news-item">
                            <div class="news-source">
                                <i class="fas fa-globe"></i>
                                <span>Reuters • 6 hours ago</span>
                            </div>
                            <div class="news-content">
                                "Institutional investors are increasing exposure to cryptocurrency assets, with Bitcoin ETFs seeing record inflows this week."
                                <span class="news-highlight">[Market Impact: Positive for Crypto]</span>
                            </div>
                        </div>
                        
                        <div class="news-item">
                            <div class="news-source">
                                <i class="fas fa-chart-line"></i>
                                <span>Market Analysis • 8 hours ago</span>
                            </div>
                            <div class="news-content">
                                "Technical indicators show strong support forming at $41,200 for Bitcoin. RSI suggests we're not in overbought territory yet."
                                <span class="news-highlight">[Market Impact: Bullish for BTC]</span>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- High Probability Trades -->
                <div class="card full-width">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fas fa-bolt"></i> High Probability Trade Signals
                        </div>
                    </div>
                    <table class="trades-table">
                        <thead>
                            <tr>
                                <th>Asset</th>
                                <th>Signal</th>
                                <th>Entry Zone</th>
                                <th>Target</th>
                                <th>Stop Loss</th>
                                <th>Probability</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>BTC/USD</td>
                                <td><span class="signal-badge signal-buy">BUY</span></td>
                                <td>$42,800 - $43,100</td>
                                <td>$45,500</td>
                                <td>$41,200</td>
                                <td>
                                    <div class="tooltip">
                                        85%
                                        <div class="tooltiptext">
                                            Based on RSI, MACD, Volume analysis and institutional inflow patterns
                                        </div>
                                    </div>
                                    <div class="progress-bar">
                                        <div class="progress-fill progress-high"></div>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td>XAU/USD</td>
                                <td><span class="signal-badge signal-buy">BUY</span></td>
                                <td>$1,975 - $1,990</td>
                                <td>$2,050</td>
                                <td>$1,940</td>
                                <td>
                                    <div class="tooltip">
                                        78%
                                        <div class="tooltiptext">
                                            Fed dovish sentiment and technical breakout pattern detected
                                        </div>
                                    </div>
                                    <div class="progress-bar">
                                        <div class="progress-fill progress-medium"></div>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td>ETH/USD</td>
                                <td><span class="signal-badge signal-hold">HOLD</span></td>
                                <td>N/A</td>
                                <td>$2,600</td>
                                <td>$2,250</td>
                                <td>
                                    <div class="tooltip">
                                        65%
                                        <div class="tooltiptext">
                                            Waiting for clearer trend direction. Current consolidation pattern observed.
                                        </div>
                                    </div>
                                    <div class="progress-bar">
                                        <div class="progress-fill progress-medium"></div>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                
                <!-- Liquidity Pools -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fas fa-water"></i> Liquidity Analysis
                        </div>
                    </div>
                    <div class="liquidity-pool">
                        <div class="pool-info">
                            <div>
                                <div>BTC Buy Wall</div>
                                <div class="text-secondary">$42,800 - $43,000</div>
                            </div>
                        </div>
                        <div class="pool-chart">
                            <canvas id="btcPoolChart"></canvas>
                        </div>
                    </div>
                    <div class="liquidity-pool">
                        <div class="pool-info">
                            <div>
                                <div>BTC Sell Wall</div>
                                <div class="text-secondary">$44,500 - $45,000</div>
                            </div>
                        </div>
                        <div class="pool-chart">
                            <canvas id="btcSellPoolChart"></canvas>
                        </div>
                    </div>
                    <div class="liquidity-pool">
                        <div class="pool-info">
                            <div>
                                <div>ETH Buy Wall</div>
                                <div class="text-secondary">$2,350 - $2,380</div>
                            </div>
                        </div>
                        <div class="pool-chart">
                            <canvas id="ethPoolChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <!-- Institution Flow -->
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">
                            <i class="fas fa-building"></i> Institution Flow
                        </div>
                    </div>
                    <div class="institution-indicator">
                        <span>BTC Accumulation</span>
                        <div class="institution-bar">
                            <div class="institution-fill" style="width: 75%"></div>
                        </div>
                        <span>75%</span>
                    </div>
                    <div class="institution-indicator">
                        <span>ETH Accumulation</span>
                        <div class="institution-bar">
                            <div class="institution-fill" style="width: 60%"></div>
                        </div>
                        <span>60%</span>
                    </div>
                    <div class="institution-indicator">
                        <span>Gold ETF Inflows</span>
                        <div class="institution-bar">
                            <div class="institution-fill" style="width: 82%"></div>
                        </div>
                        <span>82%</span>
                    </div>
                    <div class="card-header" style="margin-top: 20px;">
                        <div class="card-title">
                            Next Expected Move
                        </div>
                    </div>
                    <div style="margin-top: 10px;">
                        <div>Large BTC buy orders detected at $42,800. Institutions likely accumulating.</div>
                        <div style="margin-top: 10px; font-size: 0.9rem; color: var(--text-secondary);">
                            Expected entry completion: 2-3 days
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        // Initialize charts
        document.addEventListener('DOMContentLoaded', function() {
            // BTC Price Chart
            const btcCtx = document.getElementById('btcChart').getContext('2d');
            const btcChart = new Chart(btcCtx, {
                type: 'line',
                data: {
                    labels: ['9:00', '10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00'],
                    datasets: [{
                        label: 'BTC Price',
                        data: [42800, 42950, 43100, 43200, 43150, 43250, 43300, 43250],
                        borderColor: '#00b894',
                        backgroundColor: 'rgba(0, 184, 148, 0.1)',
                        borderWidth: 2,
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            grid: {
                                display: false
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        },
                        y: {
                            grid: {
                                color: 'rgba(255,255,255,0.1)'
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        }
                    }
                }
            });
            
            // ETH Price Chart
            const ethCtx = document.getElementById('ethChart').getContext('2d');
            const ethChart = new Chart(ethCtx, {
                type: 'line',
                data: {
                    labels: ['9:00', '10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00'],
                    datasets: [{
                        label: 'ETH Price',
                        data: [2350, 2360, 2370, 2375, 2370, 2378, 2380, 2380],
                        borderColor: '#6c5ce7',
                        backgroundColor: 'rgba(108, 92, 231, 0.1)',
                        borderWidth: 2,
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            grid: {
                                display: false
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        },
                        y: {
                            grid: {
                                color: 'rgba(255,255,255,0.1)'
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        }
                    }
                }
            });
            
            // Gold Price Chart
            const goldCtx = document.getElementById('goldChart').getContext('2d');
            const goldChart = new Chart(goldCtx, {
                type: 'line',
                data: {
                    labels: ['9:00', '10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00'],
                    datasets: [{
                        label: 'Gold Price',
                        data: [1975, 1978, 1980, 1982, 1983, 1984, 1985, 1985],
                        borderColor: '#fdcb6e',
                        backgroundColor: 'rgba(253, 203, 110, 0.1)',
                        borderWidth: 2,
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            grid: {
                                display: false
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        },
                        y: {
                            grid: {
                                color: 'rgba(255,255,255,0.1)'
                            },
                            ticks: {
                                color: 'rgba(255,255,255,0.7)'
                            }
                        }
                    }
                }
            });
            
            // Liquidity Pool Charts
            const poolCtx1 = document.getElementById('btcPoolChart').getContext('2d');
            const poolChart1 = new Chart(poolCtx1, {
                type: 'bar',
                data: {
                    labels: [''],
                    datasets: [{
                        label: 'Buy Orders',
                        data: [85],
                        backgroundColor: 'rgba(0, 184, 148, 0.8)',
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            display: false
                        },
                        y: {
                            display: false,
                            beginAtZero: true
                        }
                    }
                }
            });
            
            const poolCtx2 = document.getElementById('btcSellPoolChart').getContext('2d');
            const poolChart2 = new Chart(poolCtx2, {
                type: 'bar',
                data: {
                    labels: [''],
                    datasets: [{
                        label: 'Sell Orders',
                        data: [65],
                        backgroundColor: 'rgba(232, 67, 147, 0.8)',
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            display: false
                        },
                        y: {
                            display: false,
                            beginAtZero: true
                        }
                    }
                }
            });
            
            const poolCtx3 = document.getElementById('ethPoolChart').getContext('2d');
            const poolChart3 = new Chart(poolCtx3, {
                type: 'bar',
                data: {
                    labels: [''],
                    datasets: [{
                        label: 'Buy Orders',
                        data: [72],
                        backgroundColor: 'rgba(108, 92, 231, 0.8)',
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        x: {
                            display: false
                        },
                        y: {
                            display: false,
                            beginAtZero: true
                        }
                    }
                }
            });
            
            // Simulate real-time updates
            setInterval(() => {
                // Update prices randomly
                const btcElement = document.querySelector('.card:nth-child(1) .price-up');
                const ethElement = document.querySelector('.card:nth-child(2) .price-up');
                const goldElement = document.querySelector('.card:nth-child(3) .price-up');
                
                // Small random price fluctuations
                const btcChange = (Math.random() - 0.5) * 100;
                const ethChange = (Math.random() - 0.5) * 20;
                const goldChange = (Math.random() - 0.5) * 5;
                
                // Update displayed prices (in a real app, this would come from an API)
                updatePriceDisplay(btcElement, 43250, btcChange);
                updatePriceDisplay(ethElement, 2380, ethChange);
                updatePriceDisplay(goldElement, 1985, goldChange);
                
                // Update chart data
                updateChartData(btcChart, btcChange / 1000);
                updateChartData(ethChart, ethChange / 100);
                updateChartData(goldChart, goldChange / 10);
                
            }, 5000);
            
            function updatePriceDisplay(element, basePrice, change) {
                const newPrice = basePrice + change;
                const percentChange = (change / basePrice * 100).toFixed(2);
                const isPositive = change >= 0;
                
                element.innerHTML = `$${newPrice.toFixed(2)} <span>${isPositive ? '+' : ''}${percentChange}%</span>`;
                element.className = isPositive ? 'price-up' : 'price-down';
            }
            
            function updateChartData(chart, change) {
                const data = chart.data.datasets[0].data;
                const lastValue = data[data.length - 1];
                const newValue = lastValue + change;
                
                // Remove first data point and add new one
                data.shift();
                data.push(newValue);
                
                chart.update('none');
            }
        });
    </script>
</body>
</html>
