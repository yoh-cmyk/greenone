<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>방문자 카운터</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            width: 393px;
            height: 852px;
            background: #000000;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            font-family: 'Arial', sans-serif;
            overflow-y: auto;
            color: #00FF00;
            padding: 40px 20px 30px 20px;
        }

        .top-section {
            text-align: center;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            padding-top: 40px;
        }

        .title {
            color: #00FF00;
            font-size: 24px;
            font-weight: normal;
            margin-bottom: 80px;
            text-align: left;
            width: 100%;
            padding-left: 15px;
        }

        .counter-section {
            flex: 2;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }

        .counter-number {
            color: #00FF00;
            font-size: 180px;
            font-weight: bold;
            line-height: 1;
            margin-bottom: 20px;
        }

        .counter-suffix {
            color: #00FF00;
            font-size: 28px;
            font-weight: normal;
            margin-bottom: 40px;
            text-align: right;
            width: 100%;
            padding-right: 20px;
        }

        .description {
            color: #00FF00;
            font-size: 18px;
            font-weight: normal;
            margin-bottom: 60px;
            line-height: 1.4;
        }

        .bottom-section {
            flex: 2;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            text-align: center;
        }

        .personal-message {
            color: #00FF00;
            font-size: 14px;
            line-height: 1.6;
            margin-bottom: 30px;
            text-align: left;
            padding: 0 10px;
        }

        .message-title {
            color: #00FF00;
            font-size: 16px;
            margin-bottom: 15px;
            text-align: center;
        }

        .timestamp-label {
            color: #00FF00;
            font-size: 16px;
            margin-bottom: 15px;
        }

        .timestamp {
            color: #00FF00;
            font-size: 14px;
            opacity: 0.8;
        }

        /* @ 모양 리셋 버튼 */
        .reset-button {
            color: #00FF00;
            font-size: 24px;
            font-weight: bold;
            background: transparent;
            border: 2px solid transparent;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s ease;
        }

        .reset-button:hover {
            background: #00FF00;
            color: #000000;
            transform: scale(1.1);
        }

        .reset-button:active {
            transform: scale(0.95);
        }

        /* 애니메이션 효과 */
        .counter-number.updated {
            animation: pulse 0.6s ease;
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
                opacity: 1;
            }
            50% {
                transform: scale(1.05);
                opacity: 0.8;
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <div class="top-section">
        <div class="title">당신은...</div>
    </div>

    <div class="counter-section">
        <div class="counter-number" id="counter">0</div>
        <div class="counter-suffix">번째</div>
        <div class="description">안전 추구형 방문자 입니다.</div>
    </div>

    <div class="bottom-section">
        <div class="message-title">당신을 위한 속마음 한 마디:</div>
        <div class="personal-message">
            당신은 위험과 온갖 논란, 갈등을 인지함에도 회피를 선택했습니다. 외부의 수많은 사람들은 각자 자신의 모습으로 활동하기를 선택하는데 당신은 작은 알고리즘에서조차 편안하기를 선택하시는군요. 마주침이란 행동의 작은 동기가 되기도 합니다. 누군가의 손으로 고치는 알고리즘이 당신이 맞을지요. 알고있다는 것만으로는 부족하죠. 당신의 화면을 채우는 것을 당신의 손으로 당신만의 것으로 만드는 것은 어떤 가요? 회피가 언제까지고 당신을 편안하게 만들어주지는 못할 거 같은데요? 직접 움직이는 것이 좋을 거에요.
        </div>
        <div class="timestamp-label">기록 (한국 시간 기준)</div>
        <div class="timestamp" id="lastVisit"></div>
        <button class="reset-button" onclick="resetCounter()" title="방문자 수 초기화">@</button>
    </div>

    <script>
        // 방문자 카운터 관리
        function updateCounter() {
            // localStorage에서 현재 카운트 가져오기
            let currentCount = localStorage.getItem('visitorCount');
            
            if (currentCount === null) {
                currentCount = 0;
            } else {
                currentCount = parseInt(currentCount);
            }
            
            // 카운트 증가
            currentCount++;
            
            // localStorage에 저장
            localStorage.setItem('visitorCount', currentCount);
            
            // 화면에 표시
            const counterElement = document.getElementById('counter');
            counterElement.textContent = currentCount.toLocaleString();
            
            // 애니메이션 효과 추가
            counterElement.classList.add('updated');
            setTimeout(() => {
                counterElement.classList.remove('updated');
            }, 600);
            
            // 마지막 방문 시간 업데이트
            const now = new Date();
            const timestamp = now.toLocaleString('ko-KR', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit'
            });
            
            localStorage.setItem('lastVisit', timestamp);
            document.getElementById('lastVisit').textContent = timestamp;
        }
        
        // 카운터 초기화 함수
        function resetCounter() {
            if (confirm('정말로 카운터를 초기화하시겠습니까?')) {
                localStorage.setItem('visitorCount', '0');
                document.getElementById('counter').textContent = '0';
                localStorage.removeItem('lastVisit');
                document.getElementById('lastVisit').textContent = '';
                
                // 페이지 새로고침으로 카운터 재시작
                setTimeout(() => {
                    location.reload();
                }, 100);
            }
        }
        
        // 페이지 로드시 실행
        window.addEventListener('load', function() {
            updateCounter();
        });
        
        // 기존 방문 시간이 있으면 표시 (새로고침 시)
        window.addEventListener('DOMContentLoaded', function() {
            const savedTime = localStorage.getItem('lastVisit');
            if (savedTime) {
                document.getElementById('lastVisit').textContent = savedTime;
            }
        });
    </script>
</body>
</html>
