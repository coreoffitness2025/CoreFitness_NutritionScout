<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nutrition Scout - 영양정보 검색 도구</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Segoe UI', 'Noto Sans KR', sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: auto;
            padding: 20px;
        }
        header {
            text-align: center;
            margin-bottom: 30px;
        }
        header h1 {
            font-size: 2rem;
            color: #4CAF50;
        }
        header p {
            color: #555;
            font-size: 1rem;
        }
        .search-container {
            display: flex;
            flex-direction: row;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        .search-input {
            flex: 1;
            padding: 12px;
            font-size: 1rem;
            border: 1px solid #ccc;
            border-radius: 6px;
        }
        .search-button {
            padding: 12px 20px;
            font-size: 1rem;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }
        .autocomplete-container {
            position: absolute;
            background-color: white;
            border: 1px solid #ccc;
            border-radius: 4px;
            max-height: 200px;
            overflow-y: auto;
            width: calc(100% - 40px);
            z-index: 1000;
        }
        .autocomplete-item {
            padding: 10px;
            cursor: pointer;
        }
        .autocomplete-item:hover {
            background-color: #eee;
        }
        .result-container {
            display: none;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            padding: 20px;
        }
        .food-name {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 10px;
        }
        .nutrition-item {
            margin-bottom: 8px;
        }
        .comment-container {
            background-color: #f0f0f0;
            padding: 15px;
            border-left: 4px solid #4CAF50;
            border-radius: 6px;
            margin-top: 20px;
        }
        @media (max-width: 600px) {
            .search-container {
                flex-direction: column;
                align-items: stretch;
            }
            .search-button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Nutrition Scout</h1>
            <p>건강한 식습관을 위한 영양정보 검색 도구</p>
        </header>
        <div class="search-container">
            <input type="text" id="searchInput" class="search-input" placeholder="음식 이름을 입력하세요">
            <button class="search-button" onclick="searchFood()">검색</button>
            <div id="autocomplete" class="autocomplete-container"></div>
        </div>
        <div id="resultContainer" class="result-container">
            <div class="food-name" id="foodName"></div>
            <div class="nutrition-item">탄수화물: <span id="carbs"></span> g/100g</div>
            <div class="nutrition-item">단백질: <span id="protein"></span> g/100g</div>
            <div class="nutrition-item">지방: <span id="fat"></span> g/100g</div>
            <div class="comment-container" id="comment"></div>
        </div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <script>
        let foodData = [];
        document.addEventListener('DOMContentLoaded', () => {
            Papa.parse('nutrition_db.csv', {
                download: true,
                header: true,
                complete: (results) => {
                    foodData = results.data.filter(item => item.요리명);
                }
            });

            const input = document.getElementById('searchInput');
            const autoBox = document.getElementById('autocomplete');
            input.addEventListener('input', () => {
                const query = input.value.trim().toLowerCase();
                autoBox.innerHTML = '';
                if (!query) return;
                const suggestions = foodData.filter(item => item.요리명.toLowerCase().includes(query)).slice(0, 10);
                suggestions.forEach(item => {
                    const div = document.createElement('div');
                    div.className = 'autocomplete-item';
                    div.textContent = item.요리명;
                    div.onclick = () => {
                        input.value = item.요리명;
                        autoBox.innerHTML = '';
                        displayResult(item);
                    };
                    autoBox.appendChild(div);
                });
            });
        });

        function searchFood() {
            const query = document.getElementById('searchInput').value.trim().toLowerCase();
            const match = foodData.find(item => item.요리명.toLowerCase() === query);
            if (match) displayResult(match);
        }

        function displayResult(food) {
            document.getElementById('foodName').textContent = food.요리명;
            document.getElementById('carbs').textContent = food['탄수화물(g/100g)'];
            document.getElementById('protein').textContent = food['단백질(g/100g)'];
            document.getElementById('fat').textContent = food['지방(g/100g)'];
            const comment = food['코멘트'] ? food['코멘트'].replace(/^.*?,\s*/, '') : '';
            document.getElementById('comment').innerHTML = comment.replace(/\n/g, '<br>');
            document.getElementById('resultContainer').style.display = 'block';
        }
    </script>
</body>
</html>
