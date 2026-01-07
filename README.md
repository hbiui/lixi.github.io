<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>银行存款利息计算器 - 可折叠汇率面板</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            color: #333;
        }
        
        .container {
            max-width: 1000px;
            width: 100%;
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            
        }
        
        .header {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            padding: 25px;
            text-align: center;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        
        .header-content {
            flex: 1;
            min-width: 300px;
        }
        
        .header h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }
        
        .header h1 i {
            font-size: 2rem;
        }
        
        .header p {
            opacity: 0.9;
            font-size: 1.1rem;
        }
        
        .header-buttons {
            display: flex;
            gap: 15px;
            margin-top: 15px;
            justify-content: center;
        }
        
        .history-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            border: 2px solid rgba(255, 255, 255, 0.3);
            padding: 12px 20px;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
            white-space: nowrap;
        }
        
        .history-btn:hover {
            background-color: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }
        
        .exchange-toggle-btn {
            background: rgba(255, 255, 255, 0.95);
            color: #2c3e50;
            border: 2px solid rgba(255, 255, 255, 0.3);
            padding: 12px 20px;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
            white-space: nowrap;
        }
        
        .exchange-toggle-btn:hover {
            background: white;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .exchange-toggle-btn i {
            color: #3498db;
            transition: transform 0.3s;
        }
        
        .exchange-toggle-btn.collapsed i {
            transform: rotate(180deg);
        }
        
        .calculator {
            display: flex;
            flex-wrap: wrap;
            padding: 0;
        }
        
        .input-section {
            flex: 1;
            min-width: 350px;
            padding: 35px;
            background-color: #f9fafc;
        }
        
        .result-section {
            flex: 1;
            min-width: 350px;
            padding: 35px;
            background-color: #ffffff;
            border-left: 1px solid #eee;
            position: relative;
            overflow: hidden;
        }
        
        .exchange-panel {
            position: absolute;
            top: 20px;
            right: 180px;
            background: rgba(255, 255, 255, 0.98);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            z-index: 99;
            border: 1px solid #e1e5eb;
            backdrop-filter: blur(10px);
            max-width: 380px;
            width: 100%;
            max-height: 0;
            overflow: hidden;
            transition: all 0.4s ease;
            opacity: 0;
        }
        
        .exchange-panel.expanded {
            max-height: 1000px;
            opacity: 1;
            padding: 20px;
        }
        
        .section-title {
            font-size: 1.5rem;
            color: #2c3e50;
            margin-bottom: 25px;
            padding-bottom: 10px;
            border-bottom: 2px solid #3498db;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .section-title i {
            color: #3498db;
        }
        
        .form-group {
            margin-bottom: 25px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #444;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        label i {
            color: #3498db;
            width: 20px;
        }
        
        .rate-info {
            font-size: 0.9rem;
            color: #666;
            margin-top: 5px;
            display: flex;
            justify-content: space-between;
            padding: 0 5px;
        }
        
        .benchmark-rate {
            color: #e74c3c;
            font-weight: 600;
        }
        
        select, input {
            width: 100%;
            padding: 14px 16px;
            border: 2px solid #e1e5eb;
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s;
            background-color: white;
        }
        
        select:focus, input:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
        }
        
        .buttons {
            display: flex;
            gap: 15px;
            margin-top: 35px;
        }
        
        button {
            flex: 1;
            padding: 16px;
            border: none;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .submit-btn {
            background-color: #3498db;
            color: white;
        }
        
        .submit-btn:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
        }
        
        .reset-btn {
            background-color: #f8f9fa;
            color: #555;
            border: 2px solid #e1e5eb;
        }
        
        .reset-btn:hover {
            background-color: #e9ecef;
            transform: translateY(-2px);
        }
        
        .result-item {
            margin-bottom: 25px;
            padding: 18px;
            background-color: #f8fafd;
            border-radius: 12px;
            border-left: 4px solid #3498db;
        }
        
        .result-label {
            font-size: 1rem;
            color: #666;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .result-value {
            font-size: 1.6rem;
            font-weight: 700;
            color: #2c3e50;
            word-break: break-all;
        }
        
        .result-value.chinese {
            font-size: 1.3rem;
            color: #e74c3c;
            font-weight: 600;
        }
        
        .highlight {
            color: #e74c3c;
        }
        
        .footer {
            text-align: center;
            padding: 20px;
            background-color: #f8f9fa;
            color: #666;
            border-top: 1px solid #eee;
            font-size: 0.9rem;
        }
        
        .footer a {
            color: #3498db;
            text-decoration: none;
        }
        
        .footer a:hover {
            text-decoration: underline;
        }
        
        /* 历史记录模态框样式 */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s;
        }
        
        .modal-overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .modal {
            background-color: white;
            border-radius: 15px;
            width: 90%;
            max-width: 800px;
            max-height: 80vh;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            transform: translateY(30px);
            transition: transform 0.3s;
        }
        
        .modal-overlay.active .modal {
            transform: translateY(0);
        }
        
        .modal-header {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .modal-header h2 {
            font-size: 1.8rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .close-modal {
            background: none;
            border: none;
            color: white;
            font-size: 1.8rem;
            cursor: pointer;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background-color 0.3s;
        }
        
        .close-modal:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        .modal-content {
            padding: 20px;
            max-height: 60vh;
            overflow-y: auto;
        }
        
        .history-list {
            list-style: none;
        }
        
        .history-item {
            padding: 18px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: background-color 0.2s;
        }
        
        .history-item:hover {
            background-color: #f9f9f9;
        }
        
        .history-info {
            flex: 1;
        }
        
        .history-main {
            font-weight: 600;
            font-size: 1.1rem;
            margin-bottom: 5px;
            color: #2c3e50;
        }
        
        .history-details {
            font-size: 0.9rem;
            color: #666;
        }
        
        .history-amount {
            font-weight: 700;
            font-size: 1.3rem;
            color: #e74c3c;
            margin-left: 15px;
        }
        
        .no-history {
            text-align: center;
            padding: 40px;
            color: #999;
            font-size: 1.1rem;
        }
        
        .clear-history {
            background-color: #e74c3c;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            margin: 20px auto 0;
            transition: background-color 0.3s;
        }
        
        .clear-history:hover {
            background-color: #c0392b;
        }
        
        .exchange-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
        }
        
        .exchange-header h3 {
            font-size: 1.2rem;
            color: #2c3e50;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .exchange-header h3 i {
            color: #3498db;
        }
        
        .refresh-btn {
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            padding: 6px 12px;
            font-size: 0.85rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
            transition: all 0.3s;
        }
        
        .refresh-btn:hover {
            background-color: #2980b9;
            transform: translateY(-1px);
        }
        
        .refresh-btn:active {
            transform: translateY(0);
        }
        
        .refresh-btn.refreshing i {
            animation: spin 1s linear infinite;
        }
        
        .exchange-time {
            font-size: 0.85rem;
            color: #666;
            margin-bottom: 15px;
            text-align: center;
            background-color: #f8f9fa;
            padding: 8px;
            border-radius: 6px;
        }
        
        .exchange-time span {
            font-weight: 600;
            color: #e74c3c;
        }
        
        /* 竖排汇率显示样式 */
        .exchange-rates-vertical {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .exchange-rate-item-vertical {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px;
            background-color: #f8f9fa;
            border-radius: 8px;
            border: 1px solid #e9ecef;
            transition: all 0.2s;
        }
        
        .exchange-rate-item-vertical:hover {
            background-color: #e9ecef;
            transform: translateX(3px);
        }
        
        .currency-info {
            display: flex;
            align-items: center;
            gap: 12px;
            flex: 1;
        }
        
        .currency-flag {
            width: 32px;
            height: 22px;
            border-radius: 3px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 0.9rem;
            color: white;
            flex-shrink: 0;
        }
        
        .currency-flag.usd {
            background: linear-gradient(to right, #002868 33%, white 33%, white 66%, #bf0a30 66%);
            color: #002868;
        }
        
        .currency-flag.eur {
            background: linear-gradient(to right, #003399 33%, #FFCC00 33%, #FFCC00 66%, #FF0000 66%);
            color: white;
        }
        
        .currency-flag.jpy {
            background-color: white;
            color: #bc002d;
            border: 1px solid #e0e0e0;
        }
        
        .currency-flag.krw {
            background: linear-gradient(to bottom, #CD2E3A 50%, #0047A0 50%);
            color: white;
        }
        
        .currency-flag.hkd {
            background: linear-gradient(to right, #EC1C24 33%, white 33%, white 66%, #EC1C24 66%);
            color: #EC1C24;
        }
        
        .currency-details {
            display: flex;
            flex-direction: column;
        }
        
        .currency-code-name {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 4px;
        }
        
        .currency-code {
            font-weight: 700;
            color: #2c3e50;
            font-size: 1rem;
        }
        
        .currency-name {
            font-size: 0.85rem;
            color: #666;
        }
        
        .currency-rate {
            font-size: 0.9rem;
            color: #666;
        }
        
        .rate-value-change {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 4px;
        }
        
        .exchange-rate-value {
            font-weight: 700;
            color: #e74c3c;
            font-size: 1.1rem;
        }
        
        .exchange-rate-change {
            font-size: 0.8rem;
            padding: 3px 8px;
            border-radius: 10px;
        }
        
        .exchange-rate-change.up {
            background-color: rgba(46, 204, 113, 0.2);
            color: #27ae60;
        }
        
        .exchange-rate-change.down {
            background-color: rgba(231, 76, 60, 0.2);
            color: #c0392b;
        }
        
        /* API设置样式 - 集成在汇率面板内 */
        .api-settings-section {
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #eee;
        }
        
        .api-settings-toggle {
            width: 100%;
            background-color: #f8f9fa;
            color: #6c757d;
            border: 1px solid #e9ecef;
            border-radius: 8px;
            padding: 10px 15px;
            font-size: 0.9rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: all 0.3s;
            margin-bottom: 10px;
        }
        
        .api-settings-toggle:hover {
            background-color: #e9ecef;
            color: #495057;
        }
        
        .api-settings-toggle i {
            transition: transform 0.3s;
        }
        
        .api-settings-toggle.collapsed i {
            transform: rotate(180deg);
        }
        
        .api-settings-content {
            background-color: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            margin-top: 10px;
            border: 1px solid #e9ecef;
            display: none;
        }
        
        .api-settings-content.expanded {
            display: block;
            animation: fadeIn 0.3s ease-out;
        }
        
        .api-form-group {
            margin-bottom: 15px;
        }
        
        .api-form-group label {
            font-size: 0.85rem;
            font-weight: 600;
            color: #444;
            margin-bottom: 5px;
            display: block;
        }
        
        .api-form-group select {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid #ced4da;
            border-radius: 6px;
            font-size: 0.9rem;
            transition: all 0.3s;
            background-color: white;
        }
        
        .api-form-group select:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
        }
        
        .api-key-input {
            display: flex;
            gap: 10px;
        }
        
        .api-key-input input {
            flex: 1;
            padding: 10px 12px;
            border: 1px solid #ced4da;
            border-radius: 6px;
            font-size: 0.9rem;
        }
        
        .api-key-input input:focus {
            outline: none;
            border-color: #3498db;
        }
        
        .api-test-btn {
            background-color: #6c757d;
            color: white;
            border: none;
            border-radius: 6px;
            padding: 10px 12px;
            font-size: 0.85rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
            transition: all 0.3s;
            white-space: nowrap;
        }
        
        .api-test-btn:hover {
            background-color: #5a6268;
        }
        
        .api-test-btn.testing i {
            animation: spin 1s linear infinite;
        }
        
        .api-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .api-save-btn {
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            padding: 10px 12px;
            font-size: 0.85rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
            transition: all 0.3s;
            flex: 1;
            justify-content: center;
        }
        
        .api-save-btn:hover {
            background-color: #2980b9;
        }
        
        .api-status {
            margin-top: 10px;
            padding: 10px;
            border-radius: 6px;
            font-size: 0.8rem;
            display: none;
        }
        
        .api-status.success {
            background-color: rgba(46, 204, 113, 0.1);
            color: #27ae60;
            border: 1px solid rgba(46, 204, 113, 0.2);
            display: block;
        }
        
        .api-status.error {
            background-color: rgba(231, 76, 60, 0.1);
            color: #c0392b;
            border: 1px solid rgba(231, 76, 60, 0.2);
            display: block;
        }
        
        .api-status.info {
            background-color: rgba(52, 152, 219, 0.1);
            color: #2980b9;
            border: 1px solid rgba(52, 152, 219, 0.2);
            display: block;
        }
        
        .exchange-note {
            font-size: 0.75rem;
            color: #999;
            text-align: center;
            margin-top: 15px;
            font-style: italic;
        }
        
        /* 动画效果 */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-out;
        }
        
        /* 撒金币动画样式 */
        .coin {
            position: absolute;
            width: 24px;
            height: 24px;
            background: radial-gradient(circle at 30% 30%, #FFD700, #FFA500);
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
            z-index: 10;
            pointer-events: none;
        }
        
        .coin:before {
            content: "¥";
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: #8B7500;
            font-weight: bold;
            font-size: 12px;
        }
        
        @keyframes coinFall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }
        
        .coin-animation {
            animation: coinFall 1.5s ease-in forwards;
        }
        
        @media (max-width: 768px) {
            .calculator {
                flex-direction: column;
            }
            
            .result-section {
                border-left: none;
                border-top: 1px solid #eee;
            }
            
            .input-section, .result-section {
                padding: 30px 25px;
            }
            
            .header {
                flex-direction: column;
                text-align: center;
            }
            
            .header-buttons {
                flex-direction: column;
                width: 100%;
                gap: 10px;
            }
            
            .history-btn, .exchange-toggle-btn {
                width: 100%;
                justify-content: center;
            }
            
            .exchange-panel {
                position: relative;
                top: 0;
                right: 0;
                margin: 10px auto 0;
                max-width: 90%;
            }
        }
        
        @media (max-width: 480px) {
            .header h1 {
                font-size: 1.8rem;
            }
            
            .result-value {
                font-size: 1.4rem;
            }
            
            .result-value.chinese {
                font-size: 1.1rem;
            }
            
            .history-item {
                flex-direction: column;
                align-items: flex-start;
            }
            
            .history-amount {
                margin-left: 0;
                margin-top: 10px;
            }
            
            .exchange-rates-vertical {
                gap: 8px;
            }
            
            .exchange-rate-item-vertical {
                padding: 10px;
            }
            
            .api-key-input {
                flex-direction: column;
            }
            
            .api-test-btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="header-content">
                <h1><i class="fas fa-calculator"></i> 银行存款利息计算器</h1>
                <p>便捷计算您的存款利息与总金额，记录每一次计算</p>
                
                <div class="header-buttons">
                    <button class="history-btn" id="historyBtn">
                        <i class="fas fa-history"></i> 查看历史记录
                    </button>
                    
                    <button class="exchange-toggle-btn" id="exchangeToggleBtn">
                        <i class="fas fa-chevron-down"></i>
                        <span>实时汇率</span>
                    </button>
                </div>
            </div>
        </div>
        
        <!-- 汇率折叠面板 - 现在包含API设置 -->
        <div class="exchange-panel" id="exchangePanel">
            <div class="exchange-header">
                <h3><i class="fas fa-exchange-alt"></i> 实时汇率</h3>
                <button class="refresh-btn" id="refreshExchangeBtn">
                    <i class="fas fa-sync-alt"></i> 刷新
                </button>
            </div>
            <div class="exchange-time">
                更新时间: <span id="exchangeUpdateTime">2026/1/7 11:34:18</span>
                <br>
                数据源: <span id="exchangeSource">ExchangeRate-API.com</span>
            </div>
            
            <!-- 竖排汇率显示 -->
            <div class="exchange-rates-vertical">
                <div class="exchange-rate-item-vertical">
                    <div class="currency-info">
                        <div class="currency-flag usd">$</div>
                        <div class="currency-details">
                            <div class="currency-code-name">
                                <span class="currency-code">USD</span>
                                <span class="currency-name">美元</span>
                            </div>
                            <div class="currency-rate">1 CNY = </div>
                        </div>
                    </div>
                    <div class="rate-value-change">
                        <div class="exchange-rate-value">0.1400</div>
                        <div class="exchange-rate-change up">+0.02%</div>
                    </div>
                </div>
                
                <div class="exchange-rate-item-vertical">
                    <div class="currency-info">
                        <div class="currency-flag eur">€</div>
                        <div class="currency-details">
                            <div class="currency-code-name">
                                <span class="currency-code">EUR</span>
                                <span class="currency-name">欧元</span>
                            </div>
                            <div class="currency-rate">1 CNY = </div>
                        </div>
                    </div>
                    <div class="rate-value-change">
                        <div class="exchange-rate-value">0.0900</div>
                        <div class="exchange-rate-change down">-0.01%</div>
                    </div>
                </div>
                
                <div class="exchange-rate-item-vertical">
                    <div class="currency-info">
                        <div class="currency-flag jpy">¥</div>
                        <div class="currency-details">
                            <div class="currency-code-name">
                                <span class="currency-code">JPY</span>
                                <span class="currency-name">日元</span>
                            </div>
                            <div class="currency-rate">1 CNY = </div>
                        </div>
                    </div>
                    <div class="rate-value-change">
                        <div class="exchange-rate-value">14.70</div>
                        <div class="exchange-rate-change up">+0.05%</div>
                    </div>
                </div>
                
                <div class="exchange-rate-item-vertical">
                    <div class="currency-info">
                        <div class="currency-flag krw">₩</div>
                        <div class="currency-details">
                            <div class="currency-code-name">
                                <span class="currency-code">KRW</span>
                                <span class="currency-name">韩元</span>
                            </div>
                            <div class="currency-rate">1 CNY = </div>
                        </div>
                    </div>
                    <div class="rate-value-change">
                        <div class="exchange-rate-value">165.30</div>
                        <div class="exchange-rate-change down">-0.03%</div>
                    </div>
                </div>
                
                <div class="exchange-rate-item-vertical">
                    <div class="currency-info">
                        <div class="currency-flag hkd">HK$</div>
                        <div class="currency-details">
                            <div class="currency-code-name">
                                <span class="currency-code">HKD</span>
                                <span class="currency-name">港币</span>
                            </div>
                            <div class="currency-rate">1 CNY = </div>
                        </div>
                    </div>
                    <div class="rate-value-change">
                        <div class="exchange-rate-value">1.0900</div>
                        <div class="exchange-rate-change up">+0.01%</div>
                    </div>
                </div>
            </div>
            
            <!-- API设置部分 - 集成在汇率面板内 -->
            <div class="api-settings-section">
                <button class="api-settings-toggle" id="apiSettingsToggle">
                    <span><i class="fas fa-cog"></i> API设置</span>
                    <i class="fas fa-chevron-down"></i>
                </button>
                
                <div class="api-settings-content" id="apiSettingsContent">
                    <div class="api-form-group">
                        <label for="apiProvider">API提供商：</label>
                        <select id="apiProvider">
                            <option value="exchangerate-api" selected>ExchangeRate-API.com (免费)</option>
                            <option value="mock">模拟数据</option>
                            <option value="fxratesapi">FXRatesAPI.com (免费)</option>
                            <option value="freecurrencyapi">FreeCurrencyAPI.com (需密钥)</option>
                            <option value="apilayer">ExchangeRates (APILayer) (需密钥)</option>
                            <option value="itick">iTick (需密钥)</option>
                            <option value="freeforexapi">Free Forex API (免费)</option>
                            <option value="bcra">阿根廷央行 (BCRA) API (免费)</option>
                        </select>
                    </div>
                    
                    <div class="api-form-group" id="apiKeyGroup">
                        <label for="apiKey">API密钥：</label>
                        <div class="api-key-input">
                            <input type="text" id="apiKey" placeholder="请输入API密钥">
                            <button class="api-test-btn" id="testApiBtn">
                                <i class="fas fa-vial"></i> 测试
                            </button>
                        </div>
                    </div>
                    
                    <div class="api-actions">
                        <button class="api-save-btn" id="saveApiBtn">
                            <i class="fas fa-save"></i> 保存设置
                        </button>
                    </div>
                    
                    <div class="api-form-group">
                        <label>状态：</label>
                        <div class="api-status" id="apiStatus">
                            <!-- API状态信息将在这里显示 -->
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="exchange-note">
                注：汇率表示1人民币兑换相应外币的数量
            </div>
        </div>
        
        <!-- 计算器主体部分 -->
        <div class="calculator">
            <div class="input-section">
                <h2 class="section-title"><i class="fas fa-edit"></i> 存款信息</h2>
                
                <div class="form-group">
                    <label for="currency"><i class="fas fa-coins"></i> 币种：</label>
                    <select id="currency">
                        <option value="CNY" selected>人民币 (CNY)</option>
                        <option value="USD">美元 (USD)</option>
                        <option value="HKD">港币 (HKD)</option>
                        <option value="EUR">欧元 (EUR)</option>
                        <option value="JPY">日元 (JPY)</option>
                        <option value="KRW">韩元 (KRW)</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label for="depositType"><i class="fas fa-calendar-alt"></i> 期限种类：</label>
                    <select id="depositType">
                        <option value="current" selected>活期存款</option>
                        <option value="3months">定期三个月</option>
                        <option value="6months">定期六个月</option>
                        <option value="1year">定期一年</option>
                        <option value="2years">定期两年</option>
                        <option value="3years">定期三年</option>
                        <option value="5years">定期五年</option>
                    </select>
                    <div class="rate-info">
                        <span>基准利率：</span>
                        <span class="benchmark-rate" id="benchmarkRate">0.35%</span>
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="amount"><i class="fas fa-money-bill-wave"></i> 存款金额：</label>
                    <input type="number" id="amount" placeholder="请输入存款金额" min="0" step="0.01">
                </div>
                
                <div class="form-group">
                    <label for="interestRate"><i class="fas fa-percentage"></i> 年利率 (%)：</label>
                    <input type="number" id="interestRate" placeholder="请输入年利率" min="0" step="0.01">
                </div>
                
                <div class="form-group">
                    <label for="months"><i class="fas fa-clock"></i> 存期（月）：</label>
                    <input type="number" id="months" placeholder="请输入存期（月）" min="0" step="1">
                </div>
                
                <div class="buttons">
                    <button class="submit-btn" id="calculateBtn">
                        <i class="fas fa-calculator"></i> 计算利息
                    </button>
                    <button class="reset-btn" id="resetBtn">
                        <i class="fas fa-redo"></i> 重置
                    </button>
                </div>
            </div>
            
            <div class="result-section" id="resultSection">
                <h2 class="section-title"><i class="fas fa-chart-line"></i> 计算结果</h2>
                
                <div class="result-item fade-in" id="interestResult">
                    <div class="result-label"><i class="fas fa-coins"></i> 利息：</div>
                    <div class="result-value">0.00 <span id="currencySymbol">元</span></div>
                </div>
                
                <div class="result-item fade-in" id="interestChinese">
                    <div class="result-label"><i class="fas fa-font"></i> 利息（大写）：</div>
                    <div class="result-value chinese">零元整</div>
                </div>
                
                <div class="result-item fade-in" id="totalResult">
                    <div class="result-label"><i class="fas fa-wallet"></i> 本息合计：</div>
                    <div class="result-value">0.00 <span id="totalCurrencySymbol">元</span></div>
                </div>
                
                <div class="result-item fade-in" id="totalChinese">
                    <div class="result-label"><i class="fas fa-font"></i> 本息大写金额：</div>
                    <div class="result-value chinese">零元整</div>
                </div>
                
                <div class="result-item fade-in" id="summary">
                    <div class="result-label"><i class="fas fa-info-circle"></i> 计算说明：</div>
                    <div class="result-value" style="font-size: 1rem; color: #666; font-weight: normal;">
                        请填写存款信息后点击"计算利息"按钮
                    </div>
                </div>
            </div>
        </div>
        
        <div class="footer">
            <p>本计算器计算结果仅供参考，实际存款利息以银行最终计算结果为准。</p>
            <p>© 2023 银行存款利息计算器 | 增强版：历史记录 + 多币种 + 可折叠汇率面板 + 动画效果 + API接口切换</p>
        </div>
    </div>

    <!-- 历史记录模态框 -->
    <div class="modal-overlay" id="historyModal">
        <div class="modal">
            <div class="modal-header">
                <h2><i class="fas fa-history"></i> 存款计算历史记录</h2>
                <button class="close-modal" id="closeModal">&times;</button>
            </div>
            <div class="modal-content">
                <ul class="history-list" id="historyList">
                    <!-- 历史记录将动态添加到这里 -->
                </ul>
                <div class="no-history" id="noHistory">暂无历史记录，请先进行计算</div>
                <button class="clear-history" id="clearHistory">
                    <i class="fas fa-trash-alt"></i> 清空历史记录
                </button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 获取DOM元素
            const currencySelect = document.getElementById('currency');
            const depositTypeSelect = document.getElementById('depositType');
            const amountInput = document.getElementById('amount');
            const interestRateInput = document.getElementById('interestRate');
            const monthsInput = document.getElementById('months');
            const calculateBtn = document.getElementById('calculateBtn');
            const resetBtn = document.getElementById('resetBtn');
            const benchmarkRateElement = document.getElementById('benchmarkRate');
            const historyBtn = document.getElementById('historyBtn');
            const historyModal = document.getElementById('historyModal');
            const closeModal = document.getElementById('closeModal');
            const historyList = document.getElementById('historyList');
            const noHistory = document.getElementById('noHistory');
            const clearHistoryBtn = document.getElementById('clearHistory');
            const resultSection = document.getElementById('resultSection');
            const exchangeToggleBtn = document.getElementById('exchangeToggleBtn');
            const exchangePanel = document.getElementById('exchangePanel');
            const refreshExchangeBtn = document.getElementById('refreshExchangeBtn');
            const exchangeUpdateTime = document.getElementById('exchangeUpdateTime');
            const exchangeSource = document.getElementById('exchangeSource');
            
            // API设置相关元素
            const apiSettingsToggle = document.getElementById('apiSettingsToggle');
            const apiSettingsContent = document.getElementById('apiSettingsContent');
            const apiProviderSelect = document.getElementById('apiProvider');
            const apiKeyInput = document.getElementById('apiKey');
            const apiKeyGroup = document.getElementById('apiKeyGroup');
            const testApiBtn = document.getElementById('testApiBtn');
            const saveApiBtn = document.getElementById('saveApiBtn');
            const apiStatus = document.getElementById('apiStatus');
            
            // 结果元素
            const interestResult = document.querySelector('#interestResult .result-value');
            const interestChinese = document.querySelector('#interestChinese .result-value');
            const totalResult = document.querySelector('#totalResult .result-value');
            const totalChinese = document.querySelector('#totalChinese .result-value');
            const summaryText = document.querySelector('#summary .result-value');
            const currencySymbol = document.getElementById('currencySymbol');
            const totalCurrencySymbol = document.getElementById('totalCurrencySymbol');
            
            // 基准利率映射
            const benchmarkRates = {
                'current': 0.35,
                '3months': 1.35,
                '6months': 1.55,
                '1year': 1.75,
                '2years': 2.25,
                '3years': 2.75,
                '5years': 2.75
            };
            
            // 存款类型名称映射
            const depositTypeNames = {
                'current': '活期存款',
                '3months': '定期三个月',
                '6months': '定期六个月',
                '1year': '定期一年',
                '2years': '定期两年',
                '3years': '定期三年',
                '5years': '定期五年'
            };
            
            // 币种符号映射
            const currencySymbols = {
                'CNY': '元',
                'USD': '美元',
                'HKD': '港币',
                'EUR': '欧元',
                'JPY': '日元',
                'KRW': '韩元'
            };
            
            // 历史记录数据
            let historyData = JSON.parse(localStorage.getItem('depositHistory')) || [];
            
            // 汇率数据
            let exchangeRates = {
                'USD': 0.1400,
                'EUR': 0.0900,
                'JPY': 14.70,
                'KRW': 165.30,
                'HKD': 1.0900
            };
            
            // 汇率变化数据
            let exchangeChanges = {
                'USD': '+0.02%',
                'EUR': '-0.01%',
                'JPY': '+0.05%',
                'KRW': '-0.03%',
                'HKD': '+0.01%'
            };
            
            // 汇率变化方向
            let exchangeDirections = {
                'USD': 'up',
                'EUR': 'down',
                'JPY': 'up',
                'KRW': 'down',
                'HKD': 'up'
            };
            
            // API配置数据 - 修改默认提供商为exchangerate-api
            let apiConfig = JSON.parse(localStorage.getItem('exchangeApiConfig')) || {
                provider: 'exchangerate-api', // 修改为ExchangeRate-API.com
                apiKey: '',
                lastTest: null,
                lastTestSuccess: false
            };
            
            // API提供商信息
            const apiProviders = {
                'mock': {
                    name: '模拟数据',
                    requiresKey: false,
                    endpoint: null,
                    format: 'mock'
                },
                'exchangerate-api': {
                    name: 'ExchangeRate-API.com',
                    requiresKey: false,
                    endpoint: 'https://api.exchangerate-api.com/v4/latest/CNY',
                    format: 'exchangerate-api'
                },
                'fxratesapi': {
                    name: 'FXRatesAPI.com',
                    requiresKey: false,
                    endpoint: 'https://api.fxratesapi.com/latest?base=CNY',
                    format: 'fxratesapi'
                },
                'freecurrencyapi': {
                    name: 'FreeCurrencyAPI.com',
                    requiresKey: true,
                    endpoint: 'https://api.freecurrencyapi.com/v1/latest?base_currency=CNY',
                    format: 'freecurrencyapi'
                },
                'apilayer': {
                    name: 'ExchangeRates (APILayer)',
                    requiresKey: true,
                    endpoint: 'https://api.apilayer.com/exchangerates_data/latest?base=CNY',
                    format: 'apilayer'
                },
                'itick': {
                    name: 'iTick',
                    requiresKey: true,
                    endpoint: 'https://api.itick.com/v1/latest?base=CNY',
                    format: 'itick'
                },
                'freeforexapi': {
                    name: 'Free Forex API',
                    requiresKey: false,
                    endpoint: 'https://www.freeforexapi.com/api/live?pairs=CNYUSD,CNYEUR,CNYJPY,CNYKRW,CNYHKD',
                    format: 'freeforexapi'
                },
                'bcra': {
                    name: '阿根廷央行 (BCRA)',
                    requiresKey: false,
                    endpoint: 'https://api.argentinacb.com/v1/latest?base=CNY',
                    format: 'bcra'
                }
            };
            
            // 初始化默认值
            function initDefaultValues() {
                const selectedType = depositTypeSelect.value;
                interestRateInput.value = benchmarkRates[selectedType];
                benchmarkRateElement.textContent = benchmarkRates[selectedType] + '%';
                
                // 设置默认存期
                if (selectedType === 'current') {
                    monthsInput.value = '6';
                } else if (selectedType === '3months') {
                    monthsInput.value = '3';
                } else if (selectedType === '6months') {
                    monthsInput.value = '6';
                } else if (selectedType === '1year') {
                    monthsInput.value = '12';
                } else if (selectedType === '2years') {
                    monthsInput.value = '24';
                } else if (selectedType === '3years') {
                    monthsInput.value = '36';
                } else if (selectedType === '5years') {
                    monthsInput.value = '60';
                }
                
                updateCurrencySymbols();
                updateSummary();
            }
            
            // 更新币种符号
            function updateCurrencySymbols() {
                const selectedCurrency = currencySelect.value;
                currencySymbol.textContent = currencySymbols[selectedCurrency];
                totalCurrencySymbol.textContent = currencySymbols[selectedCurrency];
            }
            
            // 存款类型变化时更新默认利率和存期
            depositTypeSelect.addEventListener('change', function() {
                initDefaultValues();
            });
            
            // 币种变化时更新符号
            currencySelect.addEventListener('change', function() {
                updateCurrencySymbols();
                updateSummary();
            });
            
            // 数字转中文大写函数
            function numberToChinese(num) {
                if (num === 0) return "零元整";
                
                const chineseNumbers = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
                const chineseUnits = ['', '拾', '佰', '仟'];
                const bigUnits = ['', '万', '亿'];
                
                let integerPart = Math.floor(num);
                let decimalPart = Math.round((num - integerPart) * 100);
                
                let result = '';
                let unitIndex = 0;
                let zeroCount = 0;
                
                if (integerPart === 0) {
                    result = '零';
                } else {
                    while (integerPart > 0) {
                        let segment = integerPart % 10000;
                        if (segment === 0) {
                            zeroCount++;
                        } else {
                            if (zeroCount > 0) {
                                result = '零' + result;
                                zeroCount = 0;
                            }
                            
                            let segmentStr = '';
                            let segmentUnitIndex = 0;
                            let segmentZero = false;
                            
                            while (segment > 0) {
                                const digit = segment % 10;
                                if (digit === 0) {
                                    if (!segmentZero && segmentStr !== '') {
                                        segmentStr = chineseNumbers[0] + segmentStr;
                                        segmentZero = true;
                                    }
                                } else {
                                    segmentStr = chineseNumbers[digit] + chineseUnits[segmentUnitIndex] + segmentStr;
                                    segmentZero = false;
                                }
                                segment = Math.floor(segment / 10);
                                segmentUnitIndex++;
                            }
                            
                            result = segmentStr + bigUnits[unitIndex] + result;
                        }
                        
                        integerPart = Math.floor(integerPart / 10000);
                        unitIndex++;
                    }
                }
                
                result += '元';
                
                if (decimalPart > 0) {
                    const jiao = Math.floor(decimalPart / 10);
                    const fen = decimalPart % 10;
                    
                    if (jiao > 0) {
                        result += chineseNumbers[jiao] + '角';
                    }
                    
                    if (fen > 0) {
                        result += chineseNumbers[fen] + '分';
                    }
                } else {
                    result += '整';
                }
                
                return result;
            }
            
            // 更新计算说明
            function updateSummary() {
                const amount = parseFloat(amountInput.value) || 0;
                const rate = parseFloat(interestRateInput.value) || 0;
                const months = parseInt(monthsInput.value) || 0;
                const depositType = depositTypeSelect.value;
                const depositName = depositTypeNames[depositType];
                const currency = currencySelect.value;
                const currencyName = currencySymbols[currency];
                
                if (amount === 0 && rate === 0 && months === 0) {
                    summaryText.innerHTML = `请填写存款信息后点击"计算利息"按钮`;
                } else {
                    summaryText.innerHTML = `您选择了<span class="highlight">${depositName}</span>，存款金额为<span class="highlight">${amount.toFixed(2)}${currencyName}</span>，年利率为<span class="highlight">${rate}%</span>，存期为<span class="highlight">${months}个月</span>。`;
                }
            }
            
            // 撒金币动画
            function createCoinAnimation() {
                const container = resultSection;
                const containerRect = container.getBoundingClientRect();
                
                for (let i = 0; i < 20; i++) {
                    const coin = document.createElement('div');
                    coin.classList.add('coin');
                    
                    const startX = Math.random() * (containerRect.width - 24);
                    coin.style.left = startX + 'px';
                    coin.style.top = '-20px';
                    
                    const delay = Math.random() * 0.5;
                    coin.style.animationDelay = delay + 's';
                    
                    const size = 16 + Math.random() * 16;
                    coin.style.width = size + 'px';
                    coin.style.height = size + 'px';
                    
                    container.appendChild(coin);
                    
                    setTimeout(() => {
                        coin.classList.add('coin-animation');
                    }, 10);
                    
                    setTimeout(() => {
                        if (coin.parentNode) {
                            coin.parentNode.removeChild(coin);
                        }
                    }, 1600);
                }
            }
            
            // 保存到历史记录
            function saveToHistory(amount, rate, months, interest, total, depositType, currency) {
                const historyItem = {
                    id: Date.now(),
                    date: new Date().toLocaleString('zh-CN'),
                    amount: amount,
                    rate: rate,
                    months: months,
                    interest: interest,
                    total: total,
                    depositType: depositType,
                    currency: currency,
                    currencySymbol: currencySymbols[currency]
                };
                
                historyData.unshift(historyItem);
                
                if (historyData.length > 20) {
                    historyData = historyData.slice(0, 20);
                }
                
                localStorage.setItem('depositHistory', JSON.stringify(historyData));
                updateHistoryDisplay();
            }
            
            // 更新历史记录显示
            function updateHistoryDisplay() {
                historyList.innerHTML = '';
                
                if (historyData.length === 0) {
                    noHistory.style.display = 'block';
                    return;
                }
                
                noHistory.style.display = 'none';
                
                historyData.forEach(item => {
                    const li = document.createElement('li');
                    li.className = 'history-item';
                    
                    const depositTypeName = depositTypeNames[item.depositType];
                    
                    li.innerHTML = `
                        <div class="history-info">
                            <div class="history-main">${depositTypeName} - ${item.months}个月</div>
                            <div class="history-details">${item.date} | 利率: ${item.rate}% | 金额: ${item.amount.toFixed(2)}${item.currencySymbol}</div>
                        </div>
                        <div class="history-amount">${item.total.toFixed(2)}${item.currencySymbol}</div>
                    `;
                    
                    li.addEventListener('click', function() {
                        loadHistoryItem(item);
                        closeHistoryModal();
                    });
                    
                    historyList.appendChild(li);
                });
            }
            
            // 加载历史记录项到表单
            function loadHistoryItem(item) {
                currencySelect.value = item.currency;
                depositTypeSelect.value = item.depositType;
                amountInput.value = item.amount;
                interestRateInput.value = item.rate;
                monthsInput.value = item.months;
                
                initDefaultValues();
                calculateInterest(false);
            }
            
            // 清空历史记录
            function clearHistory() {
                if (confirm('确定要清空所有历史记录吗？此操作不可撤销。')) {
                    historyData = [];
                    localStorage.removeItem('depositHistory');
                    updateHistoryDisplay();
                }
            }
            
            // 打开历史记录模态框
            function openHistoryModal() {
                updateHistoryDisplay();
                historyModal.classList.add('active');
                document.body.style.overflow = 'hidden';
            }
            
            // 关闭历史记录模态框
            function closeHistoryModal() {
                historyModal.classList.remove('active');
                document.body.style.overflow = '';
            }
            
            // 计算利息
            function calculateInterest(saveHistory = true) {
                const amount = parseFloat(amountInput.value) || 0;
                const annualRate = parseFloat(interestRateInput.value) || 0;
                const months = parseInt(monthsInput.value) || 0;
                const depositType = depositTypeSelect.value;
                const currency = currencySelect.value;
                
                if (amount <= 0) {
                    alert('请输入有效的存款金额！');
                    amountInput.focus();
                    return;
                }
                
                if (annualRate <= 0) {
                    alert('请输入有效的年利率！');
                    interestRateInput.focus();
                    return;
                }
                
                if (months <= 0) {
                    alert('请输入有效的存期！');
                    monthsInput.focus();
                    return;
                }
                
                const interest = amount * (annualRate / 100) * (months / 12);
                const total = amount + interest;
                
                interestResult.innerHTML = `${interest.toFixed(2)} <span id="currencySymbol">${currencySymbols[currency]}</span>`;
                totalResult.innerHTML = `${total.toFixed(2)} <span id="totalCurrencySymbol">${currencySymbols[currency]}</span>`;
                
                interestChinese.textContent = numberToChinese(interest);
                totalChinese.textContent = numberToChinese(total);
                
                updateSummary();
                
                if (saveHistory) {
                    saveToHistory(amount, annualRate, months, interest, total, depositType, currency);
                    createCoinAnimation();
                }
                
                const resultItems = document.querySelectorAll('.result-item');
                resultItems.forEach(item => {
                    item.classList.remove('fade-in');
                    void item.offsetWidth;
                    item.classList.add('fade-in');
                });
            }
            
            // 重置表单
            function resetForm() {
                amountInput.value = '';
                initDefaultValues();
                
                interestResult.innerHTML = `0.00 <span id="currencySymbol">${currencySymbols[currencySelect.value]}</span>`;
                interestChinese.textContent = '零元整';
                totalResult.innerHTML = `0.00 <span id="totalCurrencySymbol">${currencySymbols[currencySelect.value]}</span>`;
                totalChinese.textContent = '零元整';
                
                updateSummary();
                
                const resultItems = document.querySelectorAll('.result-item');
                resultItems.forEach(item => {
                    item.classList.remove('fade-in');
                    void item.offsetWidth;
                    item.classList.add('fade-in');
                });
            }
            
            // 初始化API设置
            function initApiSettings() {
                apiProviderSelect.value = apiConfig.provider;
                apiKeyInput.value = apiConfig.apiKey || '';
                
                updateApiKeyVisibility();
                updateApiStatus();
                exchangeSource.textContent = apiProviders[apiConfig.provider].name;
            }
            
            // 更新API密钥输入框的可见性
            function updateApiKeyVisibility() {
                const provider = apiProviderSelect.value;
                if (apiProviders[provider].requiresKey) {
                    apiKeyGroup.style.display = 'block';
                } else {
                    apiKeyGroup.style.display = 'none';
                }
            }
            
            // 更新API状态显示
            function updateApiStatus() {
                apiStatus.className = 'api-status info';
                apiStatus.style.display = 'block';
                
                if (apiConfig.lastTest === null) {
                    apiStatus.innerHTML = `<i class="fas fa-info-circle"></i> 未测试API连接`;
                } else {
                    const date = new Date(apiConfig.lastTest);
                    const timeStr = date.toLocaleString('zh-CN');
                    
                    if (apiConfig.lastTestSuccess) {
                        apiStatus.innerHTML = `<i class="fas fa-check-circle"></i> 上次测试成功 (${timeStr})`;
                        apiStatus.className = 'api-status success';
                    } else {
                        apiStatus.innerHTML = `<i class="fas fa-times-circle"></i> 上次测试失败 (${timeStr})`;
                        apiStatus.className = 'api-status error';
                    }
                }
            }
            
            // 测试API连接
            async function testApiConnection() {
                const provider = apiProviderSelect.value;
                const apiKey = apiKeyInput.value.trim();
                const providerInfo = apiProviders[provider];
                
                testApiBtn.classList.add('testing');
                apiStatus.className = 'api-status info';
                apiStatus.innerHTML = `<i class="fas fa-spinner fa-spin"></i> 测试API连接中...`;
                
                try {
                    if (provider === 'mock') {
                        setTimeout(() => {
                            apiConfig.lastTest = new Date().toISOString();
                            apiConfig.lastTestSuccess = true;
                            apiConfig.apiKey = '';
                            updateApiStatus();
                            testApiBtn.classList.remove('testing');
                        }, 500);
                        return;
                    }
                    
                    if (providerInfo.requiresKey && !apiKey) {
                        throw new Error('此API需要提供密钥');
                    }
                    
                    let url = providerInfo.endpoint;
                    let headers = {};
                    
                    if (provider === 'freecurrencyapi') {
                        headers['apikey'] = apiKey;
                    } else if (provider === 'apilayer') {
                        headers['apikey'] = apiKey;
                    } else if (provider === 'itick') {
                        headers['Authorization'] = `Bearer ${apiKey}`;
                    }
                    
                    // 添加超时处理
                    const controller = new AbortController();
                    const timeoutId = setTimeout(() => controller.abort(), 10000); // 10秒超时
                    
                    const response = await fetch(url, { 
                        headers,
                        signal: controller.signal 
                    });
                    
                    clearTimeout(timeoutId);
                    
                    if (!response.ok) {
                        throw new Error(`HTTP错误: ${response.status}`);
                    }
                    
                    const data = await response.json();
                    
                    if (!validateApiResponse(provider, data)) {
                        throw new Error('API返回数据格式不正确');
                    }
                    
                    apiConfig.lastTest = new Date().toISOString();
                    apiConfig.lastTestSuccess = true;
                    apiConfig.apiKey = apiKey;
                    updateApiStatus();
                    
                    apiStatus.innerHTML = `<i class="fas fa-check-circle"></i> 连接成功! 获取到汇率数据`;
                    apiStatus.className = 'api-status success';
                    
                } catch (error) {
                    apiConfig.lastTest = new Date().toISOString();
                    apiConfig.lastTestSuccess = false;
                    updateApiStatus();
                    
                    let errorMessage = error.message;
                    if (error.name === 'AbortError') {
                        errorMessage = '请求超时，请检查网络连接';
                    }
                    
                    apiStatus.innerHTML = `<i class="fas fa-times-circle"></i> 连接失败: ${errorMessage}`;
                    apiStatus.className = 'api-status error';
                } finally {
                    testApiBtn.classList.remove('testing');
                    localStorage.setItem('exchangeApiConfig', JSON.stringify(apiConfig));
                }
            }
            
            // 验证API响应格式
            function validateApiResponse(provider, data) {
                switch (provider) {
                    case 'exchangerate-api':
                        return data.rates && data.base === 'CNY';
                    case 'fxratesapi':
                        return data.rates && data.base === 'CNY';
                    case 'freecurrencyapi':
                        return data.data && data.data.USD;
                    case 'apilayer':
                        return data.rates && data.base === 'CNY';
                    case 'freeforexapi':
                        return data.rates;
                    case 'bcra':
                        return data.rates;
                    default:
                        return false;
                }
            }
            
            // 保存API设置
            function saveApiSettings() {
                apiConfig.provider = apiProviderSelect.value;
                apiConfig.apiKey = apiKeyInput.value.trim();
                
                localStorage.setItem('exchangeApiConfig', JSON.stringify(apiConfig));
                
                updateApiStatus();
                exchangeSource.textContent = apiProviders[apiConfig.provider].name;
                
                const originalText = saveApiBtn.innerHTML;
                saveApiBtn.innerHTML = '<i class="fas fa-check"></i> 已保存';
                saveApiBtn.style.backgroundColor = '#27ae60';
                
                setTimeout(() => {
                    saveApiBtn.innerHTML = originalText;
                    saveApiBtn.style.backgroundColor = '';
                }, 1500);
                
                refreshExchangeRates();
            }
            
            // 切换API设置显示/隐藏
            function toggleApiSettings() {
                apiSettingsContent.classList.toggle('expanded');
                apiSettingsToggle.classList.toggle('collapsed');
                
                if (apiSettingsContent.classList.contains('expanded')) {
                    apiSettingsToggle.innerHTML = '<span><i class="fas fa-cog"></i> 收起API设置</span> <i class="fas fa-chevron-up"></i>';
                } else {
                    apiSettingsToggle.innerHTML = '<span><i class="fas fa-cog"></i> API设置</span> <i class="fas fa-chevron-down"></i>';
                }
            }
            
            // 更新汇率显示
            function updateExchangeRates() {
                const now = new Date();
                const formattedTime = now.toLocaleString('zh-CN', {
                    year: 'numeric',
                    month: '2-digit',
                    day: '2-digit',
                    hour: '2-digit',
                    minute: '2-digit',
                    second: '2-digit',
                    hour12: false
                });
                
                exchangeUpdateTime.textContent = formattedTime;
                
                document.querySelectorAll('.exchange-rate-item-vertical').forEach(item => {
                    const code = item.querySelector('.currency-code').textContent;
                    if (exchangeRates[code]) {
                        item.querySelector('.exchange-rate-value').textContent = exchangeRates[code].toFixed(4);
                        const changeElement = item.querySelector('.exchange-rate-change');
                        changeElement.textContent = exchangeChanges[code];
                        changeElement.className = `exchange-rate-change ${exchangeDirections[code]}`;
                    }
                });
            }
            
            // 从API获取汇率数据
            async function fetchExchangeRatesFromApi() {
                const provider = apiConfig.provider;
                const apiKey = apiConfig.apiKey;
                const providerInfo = apiProviders[provider];
                
                if (provider === 'mock' || !apiConfig.lastTestSuccess) {
                    simulateExchangeRateUpdate(true);
                    return false;
                }
                
                try {
                    let url = providerInfo.endpoint;
                    let headers = {};
                    
                    if (provider === 'freecurrencyapi') {
                        headers['apikey'] = apiKey;
                    } else if (provider === 'apilayer') {
                        headers['apikey'] = apiKey;
                    } else if (provider === 'itick') {
                        headers['Authorization'] = `Bearer ${apiKey}`;
                    }
                    
                    // 添加超时处理
                    const controller = new AbortController();
                    const timeoutId = setTimeout(() => controller.abort(), 10000); // 10秒超时
                    
                    const response = await fetch(url, { 
                        headers,
                        signal: controller.signal 
                    });
                    
                    clearTimeout(timeoutId);
                    
                    if (!response.ok) {
                        throw new Error(`HTTP错误: ${response.status}`);
                    }
                    
                    const data = await response.json();
                    const rates = parseApiResponse(provider, data);
                    
                    updateExchangeRatesFromApiData(rates);
                    exchangeSource.textContent = providerInfo.name;
                    
                    return true;
                    
                } catch (error) {
                    console.error('获取汇率数据失败:', error);
                    simulateExchangeRateUpdate(true);
                    exchangeSource.textContent = '模拟数据 (API失败)';
                    return false;
                }
            }
            
            // 解析API响应数据
            function parseApiResponse(provider, data) {
                const rates = {};
                
                switch (provider) {
                    case 'exchangerate-api':
                        rates.USD = data.rates.USD;
                        rates.EUR = data.rates.EUR;
                        rates.JPY = data.rates.JPY;
                        rates.KRW = data.rates.KRW;
                        rates.HKD = data.rates.HKD;
                        break;
                        
                    case 'fxratesapi':
                        rates.USD = data.rates.USD;
                        rates.EUR = data.rates.EUR;
                        rates.JPY = data.rates.JPY;
                        rates.KRW = data.rates.KRW;
                        rates.HKD = data.rates.HKD;
                        break;
                        
                    case 'freecurrencyapi':
                        rates.USD = data.data.USD;
                        rates.EUR = data.data.EUR;
                        rates.JPY = data.data.JPY;
                        rates.KRW = data.data.KRW || 165.30;
                        rates.HKD = data.data.HKD || 1.0900;
                        break;
                        
                    case 'apilayer':
                        rates.USD = data.rates.USD;
                        rates.EUR = data.rates.EUR;
                        rates.JPY = data.rates.JPY;
                        rates.KRW = data.rates.KRW;
                        rates.HKD = data.rates.HKD;
                        break;
                        
                    case 'freeforexapi':
                        if (data.rates) {
                            rates.USD = 1 / data.rates.CNYUSD.rate;
                            rates.EUR = 1 / data.rates.CNYEUR.rate;
                            rates.JPY = 1 / data.rates.CNYJPY.rate;
                            rates.KRW = 1 / data.rates.CNYKRW.rate;
                            rates.HKD = 1 / data.rates.CNYHKD.rate;
                        }
                        break;
                        
                    case 'bcra':
                        rates.USD = data.rates.USD || 0.1400;
                        rates.EUR = data.rates.EUR || 0.0900;
                        rates.JPY = data.rates.JPY || 14.70;
                        rates.KRW = data.rates.KRW || 165.30;
                        rates.HKD = data.rates.HKD || 1.0900;
                        break;
                }
                
                return rates;
            }
            
            // 从API数据更新汇率
            function updateExchangeRatesFromApiData(rates) {
                exchangeRates.USD = rates.USD || exchangeRates.USD;
                exchangeRates.EUR = rates.EUR || exchangeRates.EUR;
                exchangeRates.JPY = rates.JPY || exchangeRates.JPY;
                exchangeRates.KRW = rates.KRW || exchangeRates.KRW;
                exchangeRates.HKD = rates.HKD || exchangeRates.HKD;
                
                Object.keys(exchangeRates).forEach(currency => {
                    const oldRate = exchangeRates[currency] / (1 + (Math.random() * 0.01 - 0.005));
                    const changePercent = ((exchangeRates[currency] - oldRate) / oldRate * 100).toFixed(2);
                    
                    exchangeChanges[currency] = (parseFloat(changePercent) >= 0 ? '+' : '') + changePercent + '%';
                    exchangeDirections[currency] = parseFloat(changePercent) >= 0 ? 'up' : 'down';
                });
                
                updateExchangeRates();
            }
            
            // 模拟汇率更新
            function simulateExchangeRateUpdate(fromApi = false) {
                const currencies = ['USD', 'EUR', 'JPY', 'KRW', 'HKD'];
                
                currencies.forEach(currency => {
                    const changePercent = (Math.random() - 0.5) * 0.01;
                    const oldRate = exchangeRates[currency];
                    const newRate = oldRate * (1 + changePercent);
                    
                    exchangeRates[currency] = parseFloat(newRate.toFixed(4));
                    exchangeChanges[currency] = (changePercent > 0 ? '+' : '') + (changePercent * 100).toFixed(2) + '%';
                    exchangeDirections[currency] = changePercent >= 0 ? 'up' : 'down';
                });
                
                updateExchangeRates();
                
                if (!fromApi) {
                    exchangeSource.textContent = '模拟数据';
                }
            }
            
            // 刷新汇率
            async function refreshExchangeRates() {
                refreshExchangeBtn.classList.add('refreshing');
                
                try {
                    await fetchExchangeRatesFromApi();
                    
                    const originalText = refreshExchangeBtn.innerHTML;
                    refreshExchangeBtn.innerHTML = '<i class="fas fa-check"></i> 已更新';
                    refreshExchangeBtn.style.backgroundColor = '#27ae60';
                    
                    setTimeout(() => {
                        refreshExchangeBtn.innerHTML = originalText;
                        refreshExchangeBtn.style.backgroundColor = '';
                    }, 1500);
                } catch (error) {
                    console.error('刷新汇率失败:', error);
                } finally {
                    refreshExchangeBtn.classList.remove('refreshing');
                }
            }
            
            // 切换汇率面板
            function toggleExchangePanel() {
                exchangePanel.classList.toggle('expanded');
                exchangeToggleBtn.classList.toggle('collapsed');
                
                if (exchangePanel.classList.contains('expanded')) {
                    exchangeToggleBtn.innerHTML = '<i class="fas fa-chevron-up"></i><span>收起汇率</span>';
                } else {
                    exchangeToggleBtn.innerHTML = '<i class="fas fa-chevron-down"></i><span>实时汇率</span>';
                }
            }
            
            // 事件监听
            calculateBtn.addEventListener('click', () => calculateInterest(true));
            resetBtn.addEventListener('click', resetForm);
            historyBtn.addEventListener('click', openHistoryModal);
            closeModal.addEventListener('click', closeHistoryModal);
            clearHistoryBtn.addEventListener('click', clearHistory);
            exchangeToggleBtn.addEventListener('click', toggleExchangePanel);
            refreshExchangeBtn.addEventListener('click', refreshExchangeRates);
            
            // API设置事件监听
            apiProviderSelect.addEventListener('change', updateApiKeyVisibility);
            testApiBtn.addEventListener('click', testApiConnection);
            saveApiBtn.addEventListener('click', saveApiSettings);
            apiSettingsToggle.addEventListener('click', toggleApiSettings);
            
            // 输入变化时更新计算说明
            amountInput.addEventListener('input', updateSummary);
            interestRateInput.addEventListener('input', updateSummary);
            monthsInput.addEventListener('input', updateSummary);
            
            // 页面加载时初始化
            initDefaultValues();
            updateHistoryDisplay();
            initApiSettings();
            updateExchangeRates();
            
            // 默认展开汇率面板
            setTimeout(() => {
                toggleExchangePanel();
                
                // 页面加载后自动测试ExchangeRate-API.com连接
                setTimeout(async () => {
                    if (apiConfig.provider === 'exchangerate-api' && !apiConfig.lastTest) {
                        await testApiConnection();
                    }
                    await fetchExchangeRatesFromApi();
                }, 1000);
            }, 500);
            
            // 示例计算
            if (!localStorage.getItem('exampleShown')) {
                setTimeout(() => {
                    amountInput.value = 10000;
                    calculateInterest(false);
                    localStorage.setItem('exampleShown', 'true');
                }, 800);
            }
        });
    </script>
</body>
</html>
