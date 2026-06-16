<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body { font-family: 'Segoe UI', sans-serif; text-align: center; padding: 15px; background-color: #eef2f3; }
        .container { background: white; padding: 20px; border-radius: 20px; box-shadow: 0 10px 20px rgba(0,0,0,0.15); max-width: 400px; margin: auto; }
        
        /* Dark Mode */
        .dark-mode { background-color: #121212; color: #fff; }
        .dark-mode .container { background: #1e1e1e; color: #fff; }
        
        .developer-tag { font-size: 0.85em; color: #888; margin-bottom: 5px; }
        h2 { color: #333; margin: 0 0 10px 0; }
        .dark-mode h2 { color: #fff; }

        .top-bar { display: flex; gap: 10px; justify-content: center; margin-bottom: 15px; }
        .box-btn { padding: 8px 15px; border-radius: 8px; border: 1px solid #ddd; cursor: pointer; font-size: 0.9em; font-weight: bold; background: #f8f9fa; }
        
        input { width: 100%; padding: 12px; border-radius: 10px; border: 2px solid #ddd; margin-bottom: 10px; font-size: 1em; box-sizing: border-box; }
        button.main-btn { width: 100%; padding: 12px; border-radius: 10px; border: none; background-color: #007bff; color: white; font-weight: bold; cursor: pointer; }

        .result-box { padding: 10px; margin-top: 10px; border-radius: 10px; font-weight: 600; }
        .age-table { display: flex; justify-content: space-between; }
        .age-col { flex: 1; text-align: center; }
        /* Fix for dark mode visibility */
        .age-col { color: #555; }
        .dark-mode .age-col { color: #ccc; }
        .age-value { font-size: 1.2em; color: #1565c0; font-weight: 800; }
        .dark-mode .age-value { color: #64b5f6; }
        
        .progress-wrapper { margin-top: 5px; text-align: left; }
        .progress-container { background: #eee; border-radius: 10px; height: 12px; width: 100%; margin-top: 5px; }
        .progress-bar { background: #28a745; height: 100%; width: 0%; border-radius: 10px; }
        
        #result { background: #e3f2fd; border: 1px solid #bbdefb; }
        .dark-mode #result { background: #2c3e50; border: 1px solid #34495e; }
        #totalDays { background: #fff3e0; color: #ef6c00; }
        .dark-mode #totalDays { background: #3e2723; color: #ffab91; }
        #birthday { background: #e8f5e9; color: #2e7d32; }
        .dark-mode #birthday { background: #1b5e20; color: #a5d6a7; }
    </style>
</head>
<body>

<div class="container" id="main-container">
    <div class="developer-tag">Developed by: <strong>Mohammed Saif</strong></div>
    <h2>Age Calculator</h2>
    
    <div class="top-bar">
        <button class="box-btn" onclick="toggleTheme()">Dark Mode 🌙</button>
        <button class="box-btn" onclick="resetApp()" style="color:#dc3545;">Reset 🔄</button>
    </div>

    <input type="date" id="dob">
    <button class="main-btn" onclick="calculateAge()">Calculate Age</button>
    
    <div id="result-area" style="display:none;">
        <div class="result-box" id="result">
            <div class="age-table">
                <div class="age-col">Years<br><span class="age-value" id="res-y">0</span></div>
                <div class="age-col">Months<br><span class="age-value" id="res-m">0</span></div>
                <div class="age-col">Days<br><span class="age-value" id="res-d">0</span></div>
            </div>
        </div>
        <div class="result-box" id="totalDays">
            Total days lived: <span id="res-total">0</span>
            <div class="progress-wrapper">
                <div id="perc-text" style="font-size:0.8em;">0% Completed</div>
                <div class="progress-container"><div class="progress-bar" id="life-bar"></div></div>
            </div>
        </div>
        <div class="result-box" id="birthday">Next Birthday: <span id="res-bday">0 days</span></div>
    </div>
</div>

<script>
    function toggleTheme() {
        document.body.classList.toggle('dark-mode');
        document.getElementById('main-container').classList.toggle('dark-mode');
    }

    function resetApp() {
        localStorage.removeItem('userDob');
        document.getElementById('dob').value = '';
        document.getElementById('result-area').style.display = 'none';
    }

    function calculateAge() {
        let dobValue = document.getElementById('dob').value;
        if(!dobValue) { alert("Please select a date!"); return; }
        let dob = new Date(dobValue);
        let today = new Date();
        if (dob > today) { alert("Future date nahi chalegi!"); return; }
        localStorage.setItem('userDob', dobValue);
        document.getElementById('result-area').style.display = 'block';
        let years = today.getFullYear() - dob.getFullYear();
        let months = today.getMonth() - dob.getMonth();
        let days = today.getDate() - dob.getDate();
        if (days < 0) { months--; days += 30; }
        if (months < 0) { years--; months += 12; }
        document.getElementById('res-y').innerText = years;
        document.getElementById('res-m').innerText = months;
        document.getElementById('res-d').innerText = days;
        let diffDays = Math.ceil(Math.abs(today - dob) / (1000 * 60 * 60 * 24)); 
        document.getElementById('res-total').innerText = diffDays;
        let percent = Math.min((diffDays / 29200) * 100, 100).toFixed(1);
        document.getElementById('life-bar').style.width = percent + "%";
        document.getElementById('perc-text').innerText = percent + "% Completed";
        let nextBday = new Date(today.getFullYear(), dob.getMonth(), dob.getDate());
        if (nextBday < today) { nextBday.setFullYear(today.getFullYear() + 1); }
        document.getElementById('res-bday').innerText = Math.ceil((nextBday - today) / (1000 * 60 * 60 * 24)) + " days";
    }
    
    window.onload = function() {
        let savedDate = localStorage.getItem('userDob');
        if (savedDate) { document.getElementById('dob').value = savedDate; calculateAge(); }
    };
</script>
</body>
</html>
