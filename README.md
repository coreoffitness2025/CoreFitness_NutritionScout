<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nutrition Scout - 영양정보 검색 도구</title>
    <!-- 스타일시트는 이전 코드에서 제공됨 -->
</head>
<body>
    <!-- HTML 구조는 이전 코드에서 제공됨 -->
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 앞부분 코드는 이전 부분과 동일

            // 추천 문구 생성 함수 (이어서)
            function generateRecommendation(carbs, protein, fat, comment) {
                let recommendation = '이 음식은 ';
                
                // 영양소 비율 기반 분석
                const total = carbs + protein + fat;
                const carbsPercent = total > 0 ? (carbs / total) * 100 : 0;
                const proteinPercent = total > 0 ? (protein / total) * 100 : 0;
                const fatPercent = total > 0 ? (fat / total) * 100 : 0;
                
                // 코멘트 텍스트 기반 키워드 분석
                const lowerComment = comment.toLowerCase();
                const hasDiet = lowerComment.includes('다이어트') || lowerComment.includes('체중 감량');
                const hasProtein = lowerComment.includes('단백질') && !lowerComment.includes('단백질이 부족');
                const hasLowCarb = lowerComment.includes('저탄수화물') || lowerComment.includes('당지수가 낮');
                
                // 영양소 비율 기반 추천
                if (proteinPercent > 40 && carbsPercent < 30) {
                    recommendation += '근육 발달 및 단백질 보충에 좋은 선택입니다.';
                } else if (carbsPercent > 60 && fatPercent < 20) {
                    recommendation += '에너지 보충이 필요한, 운동 전후 식사에 적합합니다.';
                } else if (fatPercent > 40) {
                    recommendation += '지방 함량이 높으니 소량만 섭취하는 것이 좋습니다.';
                } else if (carbsPercent < 30 && fatPercent < 30 && proteinPercent > 30) {
                    recommendation += '균형 잡힌 영양소를 제공하는 건강한 선택입니다.';
                }
                // 코멘트 키워드 기반 추천
                else if (hasDiet) {
                    recommendation += '체중 관리 중인 분들에게 적절한 선택입니다.';
                } else if (hasProtein && !hasDiet) {
                    recommendation += '운동 후 근육 회복에 도움이 되는 식품입니다.';
                } else if (hasLowCarb) {
                    recommendation += '혈당 관리가 필요한 분들에게 적합한 식품입니다.';
                } else {
                    recommendation += '일반적인 식단에 포함할 수 있는 식품입니다.';
                }
                
                recommendationText.textContent = recommendation;
            }
            
            // 검색창 외부 클릭 시 자동완성 닫기
            document.addEventListener('click', function(e) {
                if (!autocompleteContainer.contains(e.target) && e.target !== searchInput) {
                    autocompleteContainer.style.display = 'none';
                }
            });

            // 모바일용 터치 이벤트 최적화
            if(/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
                // 모바일 장치에서 모든 클릭 가능한 요소에 터치 최적화
                document.querySelectorAll('button, .autocomplete-item').forEach(function(element) {
                    element.style.padding = '12px 15px'; // 터치 영역 넓히기
                });
                
                // 터치 제스처 대응 (옵션)
                let touchStartY = 0;
                document.addEventListener('touchstart', function(e) {
                    touchStartY = e.touches[0].clientY;
                });
                
                document.addEventListener('touchend', function(e) {
                    const touchEndY = e.changedTouches[0].clientY;
                    const yDiff = touchStartY - touchEndY;
                    
                    // 아래로 스와이프할 때 자동완성 닫기
                    if(Math.abs(yDiff) > 50 && yDiff < 0) {
                        autocompleteContainer.style.display = 'none';
                    }
                });
            }

            // 파싱 오류 처리 강화
            window.addEventListener('error', function(e) {
                console.error('스크립트 오류 발생:', e.message);
                if(loadingContainer.style.display === 'block') {
                    loadingContainer.style.display = 'none';
                    alert('데이터 로딩 중 오류가 발생했습니다. 페이지를 새로고침 해주세요.');
                }
            });
        });
    </script>
</body>
</html>
