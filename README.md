# web_demo.html
<!DOCTYPE html>
<html>
<head>
    <title>🔐 Password Strength Analyzer - Pro Demo</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
        }
        .container {
            background: rgba(22, 33, 62, 0.95);
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            max-width: 500px;
            width: 90%;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.1);
        }
        h1 {
            text-align: center;
            margin-bottom: 10px;
            font-size: 28px;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .subtitle {
            text-align: center;
            color: #a0a0a0;
            margin-bottom: 30px;
            font-size: 14px;
        }
        .input-group {
            position: relative;
            margin-bottom: 25px;
        }
        input[type="password"] {
            width: 100%;
            padding: 18px 20px;
            font-size: 18px;
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 15px;
            background: rgba(255,255,255,0.05);
            color: white;
            outline: none;
            transition: all 0.3s ease;
        }
        input[type="password"]:focus {
            border-color: #4ecdc4;
            box-shadow: 0 0 20px rgba(78, 205, 196, 0.3);
            background: rgba(255,255,255,0.1);
        }
        input::placeholder { color: #888; }
        button {
            width: 100%;
            padding: 18px;
            font-size: 18px;
            font-weight: bold;
            border: none;
            border-radius: 15px;
            background: linear-gradient(45deg, #0f3460, #533483);
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(15, 52, 96, 0.4);
        }
        #result {
            margin-top: 25px;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            display: none;
            animation: slideIn 0.5s ease;
        }
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .weak { background: rgba(255, 71, 87, 0.2); border: 2px solid #ff4757; color: #ff4757; }
        .strong { background: rgba(46, 213, 115, 0.2); border: 2px solid #2ed573; color: #2ed573; }
        .elite { background: rgba(255, 99, 72, 0.2); border: 2px solid #ff6348; color: #ff6348; }
        .score-bar {
            width: 100%;
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 4px;
            margin: 15px 0;
            overflow: hidden;
        }
        .score-fill {
            height: 100%;
            transition: all 0.5s ease;
            border-radius: 4px;
        }
        .issues {
            margin-top: 15px;
            text-align: left;
            font-size: 14px;
            line-height: 1.6;
        }
        .badge {
            display: inline-block;
            padding: 4px 12px;
            margin: 2px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }
        footer {
            text-align: center;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid rgba(255,255,255,0.1);
            font-size: 12px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔐 Password Strength Analyzer</h1>
        <p class="subtitle">Professional Cybersecurity Tool - Instant Analysis</p>
        
        <div class="input-group">
            <input type="password" id="pwd" placeholder="Enter password to analyze...">
        </div>
        
        <button onclick="analyze()">Analyze Strength</button>
        
        <div id="result">
            <div class="score-bar">
                <div class="score-fill" id="scoreFill" style="width: 0%"></div>
            </div>
            <div id="strengthText"></div>
            <div id="issues" class="issues"></div>
        </div>
        
        <footer>Made for cybersecurity portfolio | Zero dependencies</footer>
    </div>

    <script>
        function analyze() {
            const pwd = document.getElementById('pwd').value;
            const result = document.getElementById('result');
            const strengthText = document.getElementById('strengthText');
            const scoreFill = document.getElementById('scoreFill');
            const issuesDiv = document.getElementById('issues');
            
            if (!pwd) {
                result.style.display = 'none';
                return;
            }
            
            // Same Python logic (professional grade)
            let score = 0;
            let issues = [];
            
            // Length check
            if (pwd.length >= 12) score += 30;
            else if (pwd.length >= 8) score += 20;
            else issues.push('❌ Too short (<8 chars)');
            
            // Character variety
            const hasUpper = /[A-Z]/.test(pwd);
            const hasLower = /[a-z]/.test(pwd);
            const hasDigit = /[0-9]/.test(pwd);
            const hasSpecial = /[^A-Za-z0-9]/.test(pwd);
            
            if (hasUpper) score += 15; else issues.push('❌ No UPPERCASE');
            if (hasLower) score += 15; else issues.push('❌ No lowercase');
            if (hasDigit) score += 20; else issues.push('❌ No numbers');
            if (hasSpecial) score += 20; else issues.push('❌ No special chars');
            
            // Common patterns penalty
            const badPatterns = ['123', 'abc', 'qwerty', 'password', 'admin'];
            for (let pattern of badPatterns) {
                if (pwd.toLowerCase().includes(pattern)) {
                    score -= 25;
                    issues.push('🚨 Common pattern detected');
                    break;
                }
            }
            
            // Final strength
            let strength, colorClass;
            if (score >= 90) {
                strength = 'ELITE 💪';
                colorClass = 'elite';
                scoreFill.style.background = '#ff6348';
            } else if (score >= 70) {
                strength = 'STRONG ✅';
                colorClass = 'strong';
                scoreFill.style.background = '#2ed573';
            } else {
                strength = 'WEAK ❌';
                colorClass = 'weak';
                scoreFill.style.background = '#ff4757';
            }
            
            // Update UI
            strengthText.innerHTML = `<h2>${strength}</h2><div style="font-size:14px;opacity:0.8">Score: ${Math.max(0, score)}/100</div>`;
            scoreFill.style.width = Math.min(100, score) + '%';
            
            issuesDiv.innerHTML = issues.length ? 
                issues.map(issue => `<span class="badge" style="background:#ff4757;color:white">${issue}</span>`).join(' ') :
                '<span class="badge" style="background:#2ed573;color:white">✅ Perfect password!</span>';
            
            result.className = `result ${colorClass}`;
            result.style.display = 'block';
            result.scrollIntoView({ behavior: 'smooth' });
        }
        
        // Real-time analysis
        document.getElementById('pwd').addEventListener('input', analyze);
    </script>
</body>
</html>
