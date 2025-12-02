# haiiii,mari bermain bersama ian<!DOCTYPE html>

        .draw {
            box-shadow: 0 0 20px rgba(156, 163, 175, 0.7);
        }
    </style>
=======
        .draw {
            box-shadow: 0 0 20px rgba(156, 163, 175, 0.7);
        }
        .loading {
            opacity: 0.7;
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 0.7; }
            50% { opacity: 0.4; }
        }
    </style>
>>>>>>> REPLACE

2
```
. Then update the playGame function to show loading states at the same time:

```diff
<<<<<<< SEARCH
            // Tampilkan pilihan pemain
            const playerChoiceObj = choices.find(c => c.value === playerChoice);
            const playerChoiceElement = document.getElementById('player-choice');
            playerChoiceElement.textContent = playerChoiceObj.symbol;
            playerChoiceElement.classList.add('shake');
            
            // Hitung pilihan Ian setelah jeda
            setTimeout(() => {
                const computerChoiceObj = choices[Math.floor(Math.random() * choices.length)];
                const computerChoiceElement = document.getElementById('computer-choice');
                computerChoiceElement.textContent = computerChoiceObj.symbol;
                computerChoiceElement.classList.add('shake');
=======
            // Show both loading states
            const playerChoiceElement = document.getElementById('player-choice');
            const computerChoiceElement = document.getElementById('computer-choice');
            
            playerChoiceElement.textContent = '...';
            playerChoiceElement.classList.add('loading');
            computerChoiceElement.textContent = '...'; 
            computerChoiceElement.classList.add('loading');
            
            // Show both choices after same delay
            setTimeout(() => {
                playerChoiceElement.classList.remove('loading');
                computerChoiceElement.classList.remove('loading');
                
                const playerChoiceObj = choices.find(c => c.value === playerChoice);
                const computerChoiceObj = choices[Math.floor(Math.random() * choices.length)];
                
                playerChoiceElement.textContent = playerChoiceObj.symbol;
                computerChoiceElement.textContent = computerChoiceObj.symbol;
                
                playerChoiceElement.classList.add('shake');
                computerChoiceElement.classList.add('shake');
>>>>>>> REPLACE
```


The changes:
1. Added new CSS animations for loading state
2. Made both player and computer show loading state simultaneously
3. Both choices are revealed at the exact same time after the delay
4. Added "..." as loading indicator for both choices

This makes the game feel more fair with synchronized timing for both player and computer selections.Here is the updated HTML content after applying above edits:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Batu Kertas Gunting</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes shake {
            0%, 100% { transform: translateY(0); }
            25% { transform: translateY(-10px); }
            75% { transform: translateY(10px); }
        }
        .shake {
            animation: shake 0.5s ease-in-out;
        }
        .choice-btn {
            transition: all 0.3s ease;
        }
        .choice-btn:hover {
            transform: scale(1.1);
        }
        .win {
            box-shadow: 0 0 20px rgba(74, 222, 128, 0.7);
        }
        .lose {
            box-shadow: 0 0 20px rgba(248, 113, 113, 0.7);
        }
        .draw {
            box-shadow: 0 0 20px rgba(156, 163, 175, 0.7);
        }
        .loading {
            opacity: 0.7;
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 0.7; }
            50% { opacity: 0.4; }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col items-center justify-center p-4">
    <div class="max-w-md w-full bg-white rounded-xl shadow-lg overflow-hidden p-6">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-8">Batu Kertas Gunting</h1>
        
        <div class="flex justify-between mb-12">
            <div class="text-center">
                <h2 class="text-xl font-semibold text-gray-700">Pemain</h2>
                <div id="player-choice" class="text-6xl my-4 min-h-[84px] flex items-center justify-center">?</div>
                <div class="text-2xl font-bold text-green-500" id="player-score">0</div>
            </div>
            
            <div class="text-center">
                <h2 class="text-xl font-semibold text-gray-700">Ian</h2>
                <div id="computer-choice" class="text-6xl my-4 min-h-[84px] flex items-center justify-center">?</div>
                <div class="text-2xl font-bold text-red-500" id="computer-score">0</div>
            </div>
        </div>
        
        <div class="flex justify-center space-x-4 mb-8">
            <button class="choice-btn bg-blue-100 hover:bg-blue-200 text-blue-800 text-4xl p-4 rounded-full w-16 h-16 flex items-center justify-center" 
                    onclick="playGame('fist')" 
                    aria-label="Batu">
                🤜
            </button>
            <button class="choice-btn bg-green-100 hover:bg-green-200 text-green-800 text-4xl p-4 rounded-full w-16 h-16 flex items-center justify-center" 
                    onclick="playGame('open-hand')" 
                    aria-label="Kertas">
                🖐
            </button>
            <button class="choice-btn bg-purple-100 hover:bg-purple-200 text-purple-800 text-4xl p-4 rounded-full w-16 h-16 flex items-center justify-center" 
                    onclick="playGame('victory-hand')" 
                    aria-label="Gunting">
                ✌
            </button>
        </div>
        
        <div id="result" class="text-center text-xl font-medium text-gray-700 h-8 mb-4"></div>
        
        <button id="reset-btn" class="w-full bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg hidden" 
                onclick="resetGame()">
            Main Lagi
        </button>
        
        <div class="mt-6 text-center text-sm text-gray-500">
            <p>Pilih salah satu: Batu (🤜), Kertas (🖐), atau Gunting (✌)</p>
            <p class="mt-1">Batu > Gunting | Gunting > Kertas | Kertas > Batu</p>
        </div>
    </div>

    <script>
        let playerScore = 0;
        let computerScore = 0;
        
        const choices = [
            { symbol: '🤜', value: 'fist', name: 'Batu' },
            { symbol: '🖐', value: 'open-hand', name: 'Kertas' },
            { symbol: '✌', value: 'victory-hand', name: 'Gunting' }
        ];
        
        function playGame(playerChoice) {
            // Reset animasi dan highlight
            document.getElementById('player-choice').classList.remove('shake', 'win', 'lose', 'draw');
            document.getElementById('computer-choice').classList.remove('shake', 'win', 'lose', 'draw');
            
            // Show both loading states
            const playerChoiceElement = document.getElementById('player-choice');
            const computerChoiceElement = document.getElementById('computer-choice');
            
            playerChoiceElement.textContent = '...';
            playerChoiceElement.classList.add('loading');
            computerChoiceElement.textContent = '...'; 
            computerChoiceElement.classList.add('loading');
            
            // Show both choices after same delay
            setTimeout(() => {
                playerChoiceElement.classList.remove('loading');
                computerChoiceElement.classList.remove('loading');
                
                const playerChoiceObj = choices.find(c => c.value === playerChoice);
                const computerChoiceObj = choices[Math.floor(Math.random() * choices.length)];
                
                playerChoiceElement.textContent = playerChoiceObj.symbol;
                computerChoiceElement.textContent = computerChoiceObj.symbol;
                
                playerChoiceElement.classList.add('shake');
                computerChoiceElement.classList.add('shake');
                
                // Tentukan pemenang
                const result = determineWinner(playerChoiceObj, computerChoiceObj);
                displayResult(result, playerChoiceObj, computerChoiceObj);
            }, 500);
        }
        
        function determineWinner(player, computer) {
            if (player.value === computer.value) return 'draw';
            
            const winConditions = {
                'fist': 'victory-hand',        // Batu mengalahkan gunting
                'open-hand': 'fist',           // Kertas mengalahkan batu
                'victory-hand': 'open-hand'    // Gunting mengalahkan kertas
            };
            
            return winConditions[player.value] === computer.value ? 'win' : 'lose';
        }
        
        function displayResult(result, player, computer) {
            const resultElement = document.getElementById('result');
            const playerChoiceElement = document.getElementById('player-choice');
            const computerChoiceElement = document.getElementById('computer-choice');
            const resetBtn = document.getElementById('reset-btn');
            
            // Tambahkan class hasil
            playerChoiceElement.classList.add(result);
            computerChoiceElement.classList.add(result === 'win' ? 'lose' : result === 'lose' ? 'win' : 'draw');
            
            // Update skor dan tampilkan pesan
            if (result === 'win') {
                playerScore++;
                document.getElementById('player-score').textContent = playerScore;
                resultElement.textContent = `Kamu menang! ${player.name} mengalahkan ${computer.name}`;
            } else if (result === 'lose') {
                computerScore++;
                document.getElementById('computer-score').textContent = computerScore;
                resultElement.textContent = `Kamu kalah! ${computer.name} mengalahkan ${player.name}`;
            } else {
                resultElement.textContent = `Seri! Keduanya memilih ${player.name}`;
            }
            
            resetBtn.classList.remove('hidden');
        }
        
        function resetGame() {
            document.getElementById('player-choice').textContent = '?';
            document.getElementById('computer-choice').textContent = '?';
            document.getElementById('result').textContent = '';
            document.getElementById('reset-btn').classList.add('hidden');
            
            // Reset class
            document.getElementById('player-choice').className = 'text-6xl my-4 min-h-[84px] flex items-center justify-center';
            document.getElementById('computer-choice').className = 'text-6xl my-4 min-h-[84px] flex items-center justify-center';
        }
    </script>
</body>
</html>
```
