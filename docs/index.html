<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nutrition Scout - 영양정보 검색 도구</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    /* ------------------
       전체 레이아웃 & 폰트
    ------------------ */
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Noto Sans KR', 'Segoe UI', sans-serif;
      background: #f3f7f2;
      margin: 0;
      padding: 0;
      color: #333;
    }
    .container {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }

    /* ------------------
       헤더
    ------------------ */
    header {
      text-align: center;
      margin-bottom: 30px;
      padding: 20px 0;
    }
    header h1 {
      font-size: 2rem;
      color: #4CAF50;
      margin: 0;
      margin-bottom: 8px;
    }
    header p {
      color: #666;
      font-size: 1rem;
      margin: 0;
    }

    /* ------------------
       검색 영역
    ------------------ */
    .search-container {
      position: relative;
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
      transition: border-color 0.2s;
    }
    .search-input:focus {
      outline: none;
      border-color: #4CAF50;
    }
    .search-button {
      padding: 12px 20px;
      font-size: 1rem;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
    .search-button:hover {
      background-color: #43a047;
    }

    /* 자동완성 팝업 */
    .autocomplete-container {
      position: absolute;
      top: calc(100% + 4px);
      left: 0;
      background-color: white;
      border: 1px solid #ccc;
      border-radius: 4px;
      max-height: 200px;
      overflow-y: auto;
      width: 100%;
      z-index: 1000;
      display: none;
    }
    .autocomplete-item {
      padding: 10px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
    .autocomplete-item:hover {
      background-color: #f0f0f0;
    }

    /* ------------------
       결과 영역
    ------------------ */
    .result-container {
      display: none;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
      padding: 20px;
      animation: fadeIn 0.3s ease;
    }
    @keyframes fadeIn {
      from {opacity: 0; transform: translateY(10px);}
      to {opacity: 1; transform: translateY(0);}
    }

    .food-name {
      font-size: 1.5rem;
      font-weight: bold;
      margin-bottom: 10px;
      color: #4CAF50;
    }
    .nutrition-item {
      margin-bottom: 8px;
      font-size: 0.95rem;
      color: #333;
    }
    .nutrition-item span {
      font-weight: bold;
      color: #4CAF50;
    }

    /* 코멘트 박스 */
    .comment-container {
      background-color: #f0f0f0;
      padding: 15px;
      border-left: 4px solid #4CAF50;
      border-radius: 6px;
      margin-top: 20px;
      font-size: 0.9rem;
      line-height: 1.4;
      color: #333;
    }

    /* ------------------
       반응형
    ------------------ */
    @media (max-width: 600px) {
      .search-container {
        flex-direction: column;
        align-items: stretch;
      }
      .search-button {
        width: 100%;
      }
      .food-name {
        font-size: 1.2rem;
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

  <!-- Papa Parse -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
  <script>
    let foodData = [];

    document.addEventListener('DOMContentLoaded', () => {
      // CSV 로딩 (raw.githubusercontent.com 링크 주의!)
      Papa.parse('https://raw.githubusercontent.com/coreoffitness2025/CoreFitness_NutritionScout/main/nutrition_db.csv', {
        download: true,
        header: true,
        complete: (results) => {
          // 요리명이 있는 데이터만 필터
          foodData = results.data.filter(item => item.요리명);
        }
      });

      const input = document.getElementById('searchInput');
      const autoBox = document.getElementById('autocomplete');

      // 자동완성
      input.addEventListener('input', () => {
        const query = input.value.trim().toLowerCase();
        autoBox.innerHTML = '';

        if (!query) {
          autoBox.style.display = 'none';
          return;
        }

        const suggestions = foodData
          .filter(item => item.요리명.toLowerCase().includes(query))
          .slice(0, 10);

        suggestions.forEach(item => {
          const div = document.createElement('div');
          div.className = 'autocomplete-item';
          div.textContent = item.요리명;
          div.onclick = () => {
            input.value = item.요리명;
            autoBox.innerHTML = '';
            autoBox.style.display = 'none';
            displayResult(item);
          };
          autoBox.appendChild(div);
        });
        autoBox.style.display = suggestions.length ? 'block' : 'none';
      });
    });

    function searchFood() {
      const query = document.getElementById('searchInput').value.trim().toLowerCase();
      const match = foodData.find(item => item.요리명.toLowerCase() === query);
      if (match) {
        displayResult(match);
      }
    }

    function displayResult(food) {
      // 요리명 표시
      document.getElementById('foodName').textContent = food.요리명;
      // 탄단지
      document.getElementById('carbs').textContent = food['탄수화물(g/100g)'] || '0';
      document.getElementById('protein').textContent = food['단백질(g/100g)'] || '0';
      document.getElementById('fat').textContent = food['지방(g/100g)'] || '0';

      // 코멘트
      const comment = food['코멘트'] ? food['코멘트'].replace(/^.*?,\s*/, '') : '';
      document.getElementById('comment').innerHTML = comment.replace(/\n/g, '<br>');

      // 결과 컨테이너 표시
      document.getElementById('resultContainer').style.display = 'block';
    }
  </script>
</body>
</html>
