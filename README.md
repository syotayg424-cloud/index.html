<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>L3会員証</title>
    <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <style>
        body { font-family: sans-serif; text-align: center; background: #f9f9f9; padding-top: 50px; }
        .card { background: white; border-radius: 20px; padding: 30px; margin: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        #days-left { font-size: 80px; color: #ff4757; font-weight: bold; }
        .unit { font-size: 20px; color: #555; }
    </style>
</head>
<body>
    <div class="card">
        <h2>卒業（引退）まであと</h2>
        <div id="days-left">--</div>
        <div class="unit">日</div>
        <p id="graduation-date" style="color: #888; margin-top: 20px;">読み込み中...</p>
    </div>

    <script>
        // あなたが発行したGASのURL
        const GAS_URL = "https://script.google.com/macros/s/AKfycbzzFop7PyprYHBZEMVCCA3OU2kXMehSkXbHdYmUCTo2lR67eXxK_2K4G5DFsBJWKTdDdw/exec";

        async function initLIFF() {
            await liff.init({ liffId: "2008760618-igOldYtb" });
            if (!liff.isLoggedIn()) {
                liff.login();
            } else {
                const profile = await liff.getProfile();
                fetchData(profile.userId);
            }
        }

        async function fetchData(userId) {
            const response = await fetch(`${GAS_URL}?id=${userId}`);
            const data = await response.json();
            if (data.status !== 'not_found') {
                const gradDate = new Date(data.graduationDate);
                const diff = Math.ceil((gradDate - new Date()) / (1000 * 60 * 60 * 24));
                document.getElementById('days-left').innerText = diff;
                document.getElementById('graduation-date').innerText = "卒業予定日: " + gradDate.toLocaleDateString();
            } else {
                document.getElementById('days-left').innerText = "？";
                document.getElementById('graduation-date').innerText = "データが見つかりません";
            }
        }
        initLIFF();
    </script>
</body>
</html>
