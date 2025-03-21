<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nutrition Scout - 영양정보 검색 도구</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #4CAF50;
            --secondary-color: #2196F3;
            --accent-color: #FF9800;
            --danger-color: #f44336;
            --text-color: #333;
            --light-gray: #f5f5f5;
            --medium-gray: #e0e0e0;
            --dark-gray: #757575;
            --card-shadow: 0 4px 8px rgba(0,0,0,0.1);
            --transition-speed: 0.3s;
            --border-radius: 8px;
            --container-width: 800px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Apple SD Gothic Neo', 'Noto Sans KR', sans-serif;
            line-height: 1.6;
            color: var(--text-color);
            background-color: #f9f9f9;
            padding: 20px;
            max-width: 100%;
            overflow-x: hidden;
        }

        .container {
            width: 100%;
            max-width: var(--container-width);
            margin: 0 auto;
            padding: 15px;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            animation: fadeIn 1s;
        }

        header h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
            color: var(--primary-color);
        }

        header p {
            font-size: 1.1rem;
            color: var(--dark-gray);
        }

        .logo {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: var(--primary-color);
        }

        .search-container {
            position: relative;
            margin-bottom: 30px;
            animation: slideUp 0.5s;
            width: 100%;
        }

        .search-input {
            width: 100%;
            padding: 15px 20px;
            font-size: 1.1rem;
            border: 2px solid var(--medium-gray);
            border-radius: var(--border-radius);
            transition: all var(--transition-speed);
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            padding-right: 60px; /* 버튼을 위한 공간 */
        }

        .search-input:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.3);
        }

        .search-button {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            padding: 10px 20px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: background-color var(--transition-speed);
            z-index: 2;
        }

        .search-button:hover {
            background-color: #45a049;
        }

        .autocomplete-container {
            position: absolute;
            width: 100%;
            max-height: 250px;
            overflow-y: auto;
            background-color: white;
            border: 1px solid var(--medium-gray);
            border-top: none;
            border-radius: 0 0 var(--border-radius) var(--border-radius);
            box-shadow: var(--card-shadow);
            z-index: 100;
            display: none;
        }

        .autocomplete-item {
            padding: 12px 15px;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .autocomplete-item:hover {
            background-color: var(--light-gray);
        }

        .result-container {
            animation: fadeIn 0.5s;
            display: none;
            width: 100%;
        }

        .result-card {
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            padding: 20px;
            margin-bottom: 20px;
            transition: transform 0.3s;
            width: 100%;
        }

        .result-card:hover {
            transform: translateY(-5px);
        }

        .result-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }

        .food-icon {
            font-size: 2rem;
            margin-right: 15px;
            color: var(--accent-color);
            flex-shrink: 0;
        }

        .food-name {
            font-size: 1.6rem;
            font-weight: bold;
            color: var(--primary-color);
            word-break: keep-all;
            word-wrap: break-word;
            flex-grow: 1;
        }

        .nutrition-info {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            margin-bottom: 25px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .nutrition-item {
            flex: 1;
            min-width: 100px; /* 모바일에서 최소 너비 보장 */
            text-align: center;
            padding: 15px 10px;
            background-color: var(--light-gray);
            border-radius: var(--border-radius);
            margin: 0 5px;
        }

        .nutrition-label {
            display: block;
            font-size: 0.9rem;
            margin-bottom: 5px;
            color: var(--dark-gray);
        }

        .nutrition-value {
            font-size: 1.4rem;
            font-weight: bold;
        }

        .carbs .nutrition-value {
            color: #e67e22; /* 주황색 - 탄수화물 */
        }

        .protein .nutrition-value {
            color: #3498db; /* 파랑색 - 단백질 */
        }

        .fat .nutrition-value {
            color: #e74c3c; /* 빨강색 - 지방 */
        }

        .comment-container {
            background-color: var(--light-gray);
            border-left: 4px solid var(--primary-color);
            padding: 15px 20px;
            border-radius: 0 var(--border-radius) var(--border-radius) 0;
            margin-bottom: 20px;
            width: 100%;
        }

        .comment-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: var(--primary-color);
        }

        .comment-text {
            color: var(--text-color);
            line-height: 1.7;
            word-break: keep-all;
            word-wrap: break-word;
        }

        .recommendation {
            background-color: #e8f5e9;
            padding: 12px 15px;
            border-radius: var(--border-radius);
            font-weight: 500;
            color: var(--primary-color);
            width: 100%;
        }

        .recommendation i {
            margin-right: 8px;
        }

        .no-result {
            text-align: center;
            padding: 30px;
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            color: var(--dark-gray);
            display: none;
            width: 100%;
        }

        .no-result i {
            font-size: 3rem;
            color: var(--medium-gray);
            margin-bottom: 15px;
        }

        .nutrition-visual {
            height: 8px;
            margin-top: 5px;
            background-color: var(--light-gray);
            border-radius: 4px;
            overflow: hidden;
            position: relative;
            width: 100%;
        }

        .nutrition-bar {
            height: 100%;
            border-radius: 4px;
        }

        .loading {
            text-align: center;
            padding: 20px;
            display: none;
            width: 100%;
        }

        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: var(--primary-color);
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        .footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            color: var(--dark-gray);
            font-size: 0.9rem;
            width: 100%;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        /* 반응형 디자인 */
        @media (max-width: 768px) {
            body {
                padding: 10px;
            }
            
            .container {
                padding: 10px;
            }

            header h1 {
                font-size: 1.8rem;
            }

            header p {
                font-size: 1rem;
            }

            .search-input {
                padding: 12px 60px 12px 15px;
                font-size: 1rem;
            }

            .search-button {
                padding: 8px 15px;
                font-size: 0.9rem;
            }

            .nutrition-info {
                flex-direction: column;
                gap: 8px;
            }

            .nutrition-item {
                margin: 0;
                width: 100%;
            }

            .result-header {
                align-items: flex-start;
            }

            .food-name {
                font-size: 1.4rem;
            }

            .comment-container {
                padding: 12px;
            }

            .comment-text {
                font-size: 0.95rem;
            }

            .recommendation {
                font-size: 0.95rem;
                padding: 10px 12px;
            }
        }

        @media (max-width: 480px) {
            .container {
                padding: 8px;
            }

            .result-card {
                padding: 15px;
            }

            .result-header {
                flex-direction: row;
                text-align: left;
            }

            .food-icon {
                margin-right: 10px;
                font-size: 1.8rem;
            }

            .food-name {
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo"><i class="fas fa-search"></i></div>
            <h1>Nutrition Scout</h1>
            <p>건강한 식습관을 위한 영양정보 검색 도구</p>
        </header>

        <div class="search-container">
            <input type="text" id="food-search" class="search-input" placeholder="음식 이름을 입력하세요...">
            <button id="search-button" class="search-button">검색</button>
            <div id="autocomplete" class="autocomplete-container"></div>
        </div>

        <div id="loading" class="loading">
            <div class="spinner"></div>
            <p>데이터 로딩 중...</p>
        </div>

        <div id="no-result" class="no-result">
            <i class="fas fa-search"></i>
            <p>검색 결과가 없습니다.</p>
        </div>

        <div id="result-container" class="result-container">
            <div class="result-card">
                <div class="result-header">
                    <div id="food-icon" class="food-icon"><i class="fas fa-utensils"></i></div>
                    <h2 id="food-name" class="food-name">음식 이름</h2>
                </div>

                <div class="nutrition-info">
                    <div class="nutrition-item carbs">
                        <span class="nutrition-label">탄수화물</span>
                        <span id="carbs-value" class="nutrition-value">0</span>
                        <span class="nutrition-unit">g/100g</span>
                        <div class="nutrition-visual">
                            <div id="carbs-bar" class="nutrition-bar" style="width: 0%; background-color: #e67e22;"></div>
                        </div>
                    </div>
                    <div class="nutrition-item protein">
                        <span class="nutrition-label">단백질</span>
                        <span id="protein-value" class="nutrition-value">0</span>
                        <span class="nutrition-unit">g/100g</span>
                        <div class="nutrition-visual">
                            <div id="protein-bar" class="nutrition-bar" style="width: 0%; background-color: #3498db;"></div>
                        </div>
                    </div>
                    <div class="nutrition-item fat">
                        <span class="nutrition-label">지방</span>
                        <span id="fat-value" class="nutrition-value">0</span>
                        <span class="nutrition-unit">g/100g</span>
                        <div class="nutrition-visual">
                            <div id="fat-bar" class="nutrition-bar" style="width: 0%; background-color: #e74c3c;"></div>
                        </div>
                    </div>
                </div>

                <div class="comment-container">
                    <div class="comment-title">영양사 코멘트</div>
                    <p id="comment-text" class="comment-text">영양 정보에 대한 코멘트가 표시됩니다.</p>
                </div>

                <div id="recommendation" class="recommendation">
                    <i class="fas fa-lightbulb"></i> <span id="recommendation-text">이 음식은 균형 잡힌 식단에 적합합니다.</span>
                </div>
            </div>
        </div>

        <footer class="footer">
            <p>© 2025 Nutrition Scout | 건강한 식생활을 위한 영양 정보 제공 서비스</p>
        </footer>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 요소 참조
            const searchInput = document.getElementById('food-search');
            const searchButton = document.getElementById('search-button');
            const autocompleteContainer = document.getElementById('autocomplete');
            const resultContainer = document.getElementById('result-container');
            const noResultContainer = document.getElementById('no-result');
            const loadingContainer = document.getElementById('loading');
            
            // 결과 요소
            const foodName = document.getElementById('food-name');
            const foodIcon = document.getElementById('food-icon');
            const carbsValue = document.getElementById('carbs-value');
            const proteinValue = document.getElementById('protein-value');
            const fatValue = document.getElementById('fat-value');
            const commentText = document.getElementById('comment-text');
            const recommendationText = document.getElementById('recommendation-text');
            const carbsBar = document.getElementById('carbs-bar');
            const proteinBar = document.getElementById('protein-bar');
            const fatBar = document.getElementById('fat-bar');
            
            // 데이터 저장 변수
            let foodData = [];
            let filteredData = [];
            
            // 모바일 장치 감지
            const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);
            
            // CSV 파일 로드 및 파싱
            loadingContainer.style.display = 'block';
            
            // nutrition_db.csv 파일을 로드합니다.
            Papa.parse('nutrition_db.csv', {
                download: true,
                header: true,
                complete: function(results) {
                    foodData = results.data.filter(item => item.요리명 && item.요리명.trim() !== '');
                    console.log('총 데이터 수:', foodData.length);
                    loadingContainer.style.display = 'none';
                    
                    // 콘솔에 첫 번째 항목 확인 (디버깅용)
                    if(foodData.length > 0) {
                        console.log('첫 번째 항목 샘플:', foodData[0]);
                    }
                },
                error: function(error) {
                    console.error('CSV 파싱 오류:', error);
                    loadingContainer.style.display = 'none';
                    alert('데이터를 불러오는 데 실패했습니다. 오류: ' + error.message);
                }
            });
            
            // 검색 입력 이벤트 처리
            searchInput.addEventListener('input', function() {
                const query = this.value.trim();
                
                // 입력값이 없으면 자동완성 숨기기
                if (!query) {
                    autocompleteContainer.style.display = 'none';
                    return;
                }
                
                // 자동완성 결과 필터링
                filteredData = foodData.filter(item => 
                    item.요리명 && item.요리명.toLowerCase().includes(query.toLowerCase())
                ).slice(0, 10); // 최대 10개 결과만 표시
                
                // 자동완성 UI 업데이트
                updateAutocompleteUI(filteredData);
            });
            
            // 자동완성 UI 업데이트 함수
            function updateAutocompleteUI(results) {
                autocompleteContainer.innerHTML = '';
                
                if (results.length === 0) {
                    autocompleteContainer.style.display = 'none';
                    return;
                }
                
                results.forEach(item => {
                    const div = document.createElement('div');
                    div.className = 'autocomplete-item';
                    div.textContent = item.요리명;
                    div.addEventListener('click', function() {
                        searchInput.value = item.요리명;
                        autocompleteContainer.style.display = 'none';
                        displayResult(item);
                    });
                    autocompleteContainer.appendChild(div);
                });
                
                autocompleteContainer.style.display = 'block';
            }
            
            // 검색 버튼 클릭 이벤트
            searchButton.addEventListener('click', performSearch);
            
            // 엔터 키 이벤트
            searchInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    performSearch();
                }
            });
            
            // 검색 실행 함수
            function performSearch() {
                const query = searchInput.value.trim();
                
                if (!query) {
                    return;
                }
                
                // 자동완성 숨기기
                autocompleteContainer.style.display = 'none';
                
                // 데이터가 로드되었는지 확인
                if(foodData.length === 0) {
                    alert('데이터가 아직 로드되지 않았습니다. 잠시 후 다시 시도해주세요.');
                    return;
                }
                
                // 정확히 일치하는 항목 찾기
                const exactMatch = foodData.find(item => 
                    item.요리명 && item.요리명.toLowerCase() === query.toLowerCase()
                );
                
                if (exactMatch) {
                    displayResult(exactMatch);
                } else {
                    // 부분 일치하는 첫 번째 항목 찾기
                    const partialMatch = foodData.find(item => 
                        item.요리명 && item.요리명.toLowerCase().includes(query.toLowerCase())
                    );
                    
                    if (partialMatch) {
                        displayResult(partialMatch);
                    } else {
                        // 결과 없음 표시
                        resultContainer.style.display = 'none';
                        noResultContainer.style.display = 'block';
                    }
                }
            }
            
            // 결과 표시 함수
            function displayResult(food) {
                console.log("표시할 결과:", food); // 디버깅용
                
                // 값이 없는 경우 처리
                const carbs = parseFloat(food['탄수화물(g/100g)']) || 0;
                const protein = parseFloat(food['단백질(g/100g)']) || 0;
                const fat = parseFloat(food['지방(g/100g)']) || 0;
                const comment = food['코멘트'] || '코멘트 정보가 없습니다.';
                
                // 결과 업데이트
                foodName.textContent = food.요리명;
                
                // 아이콘 선택 (간단한 로직으로 음식 카테고리 추정)
                setFoodIcon(food.요리명, carbs, protein, fat);
                
                // 영양소 값 업데이트
                carbsValue.textContent = carbs.toFixed(1);
                proteinValue.textContent = protein.toFixed(1);
                fatValue.textContent = fat.toFixed(1);
                
                // 코멘트 업데이트 (쉼표나 마침표로 구분하여 단락 나누기)
                formatComment(comment);
                
                // 추천 문구 생성
                generateRecommendation(carbs, protein, fat, comment);
                
                // 영양소 바 시각화 (최대값 기준으로 상대적 비율 계산)
                const maxNutrient = Math.max(carbs, protein, fat, 30); // 최소 30으로 설정하여 너무 낮은 값에도 시각적 효과 유지
                carbsBar.style.width = Math.min(100, (carbs / maxNutrient * 100)) + '%';
                proteinBar.style.width = Math.min(100, (protein / maxNutrient * 100)) + '%';
                fatBar.style.width = Math.min(100, (fat / maxNutrient * 100)) + '%';
                
                // 결과 컨테이너 표시
                noResultContainer.style.display = 'none';
                resultContainer.style.display = 'block';
                
                // 모바일에서는 키보드 숨기기
                if (isMobile) {
                    searchInput.blur();
                }
                
                // 결과로 스크롤
                setTimeout(() => {
                    resultContainer.scrollIntoView({behavior: 'smooth'});
                }, 100);
            }
            
            // 음식 아이콘 설정 함수
            function setFoodIcon(foodName, carbs, protein, fat) {
                let iconClass = 'fas fa-utensils'; // 기본 아이콘
                
                // 간단한 키워드 기반 분류
                const lowerName = foodName.toLowerCase();
                
                if (lowerName.includes('밥') || lowerName.includes('면') || lowerName.includes('국수') || lowerName.includes('파스타')) {
                    iconClass = 'fas fa-bowl-rice';
                } else if (lowerName.includes('고기') || lowerName.includes('돼지') || lowerName.includes('소고기') || lowerName.includes('닭') || protein > 15) {
                    iconClass = 'fas fa-drumstick-bite';
                } else if (lowerName.includes('국') || lowerName.includes('탕') || lowerName.includes('찌개')) {
                    iconClass = 'fas fa-mug-hot';
                } else if (lowerName.includes('샐러드') || lowerName.includes('채소')) {
                    iconClass = 'fas fa-carrot';
                } else if (lowerName.includes('빵') || lowerName.includes('케이크')) {
                    iconClass = 'fas fa-bread-slice';
                } else if (lowerName.includes('과일') || lowerName.includes('사과') || lowerName.includes('바나나')) {
                    iconClass = 'fas fa-lemon';
                } else if (lowerName.includes('생선') || lowerName.includes('회') || lowerName.includes('해물')) {
                    iconClass = 'fas fa-fish';
                } else if (lowerName.includes('우유') || lowerName.includes('치즈') || lowerName.includes('요거트')) {
                    iconClass = 'fas fa-cheese';
                }
                
                // 영양소 기반 분류 (키워드 분류가 없는 경우)
                else if (carbs > fat && carbs > protein) {
                    iconClass = 'fas fa-bread-slice'; // 탄수화물이 높으면 빵 아이콘
                } else if (protein > carbs && protein > fat) {
                    iconClass = 'fas fa-drumstick-bite'; // 단백질이 높으면 고기 아이콘
                } else if (fat > carbs && fat > protein) {
                    iconClass = 'fas fa-cheese'; // 지방이 높으면 치즈 아이콘
                }
                
                foodIcon.innerHTML = `<i class="${iconClass}"></i>`;
            }
            
            // 코멘트 형식화 함수
            function formatComment(comment) {
                if (!comment) {
                    commentText.textContent = '코멘트 정보가 없습니다.';
                    return;
                }
                
                // 따옴표 제거
                let cleanComment = comment.replace(/^["']|["']$/g, '');
                
                // 요리명 뒤의 쉼표 찾기
                const commaIndex = cleanComment.indexOf(',');
                if (commaIndex > -1) {
                    cleanComment = cleanComment.substring(commaIndex + 1).trim();
                }
                
                // HTML 개행으로 변환
                commentText.innerHTML = cleanComment
                    .replace(/\.\s+/g, '.<br><br>') // 마침표 뒤 공백 있는
