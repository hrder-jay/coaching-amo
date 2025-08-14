<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>엑코치의 코칭 시뮬레이션</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://unpkg.com/docx@8.5.0/build/index.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', 'Inter', sans-serif;
        }
        .tab-active {
            background-color: #4f46e5; /* indigo-600 */
            color: white;
        }
        .tab-inactive {
            background-color: #374151; /* gray-700 */
            color: #d1d5db; /* gray-300 */
        }
        .amo-tag {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            padding: 0.25rem 0.6rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            margin-right: 0.5rem;
        }
        .amo-up { background-color: #10b981; color: white; } /* emerald-500 */
        .amo-down { background-color: #ef4444; color: white; } /* red-500 */
        .amo-neutral { background-color: #6b7280; color: white; } /* gray-500 */
        
        .modal-backdrop {
            transition: opacity 0.3s ease-in-out;
        }
        .modal-container {
            transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
        }
        .tooltip-text {
            transition: opacity 0.3s;
        }
        .feedback-suggestion-btn {
            background-color: #374151; /* gray-700 */
            border: 1px solid #4b5563; /* gray-600 */
        }
        .feedback-suggestion-btn:hover {
            background-color: #4b5563; /* gray-600 */
            border-color: #6b7280; /* gray-500 */
        }
        /* Trait Slider Radio Button Styles */
        .trait-scale {
            display: flex;
            justify-content: space-between;
            padding: 0 5px;
        }
        .trait-scale input[type="radio"] {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border: 2px solid #6b7280; /* gray-500 */
            border-radius: 50%;
            transition: all 0.2s;
            cursor: pointer;
        }
        .trait-scale input[type="radio"]:hover {
            background-color: #4b5563; /* gray-600 */
        }
        .trait-scale input[type="radio"]:checked {
            background-color: #6366f1; /* indigo-500 */
            border-color: #818cf8; /* indigo-400 */
        }
        
        /* Landing Page Animation */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .fade-in { animation: fadeIn 1s ease-in-out forwards; }
        .fade-in-up { animation: fadeInUp 1s ease-in-out forwards; }
        
        /* Toggle Switch */
        .toggle-checkbox:checked {
            right: 0;
            border-color: #4f46e5;
        }
        .toggle-checkbox:checked + .toggle-label {
            background-color: #4f46e5;
        }
    </style>
</head>
<body class="bg-gray-900 text-gray-200">

    <!-- Landing Page -->
    <div id="landing-page" class="relative h-screen w-full flex items-center justify-center overflow-hidden">
        <div class="absolute inset-0 z-0">
            <img src="https://images.unsplash.com/photo-1521737604893-d14cc237f11d?q=80&w=2084&auto=format&fit=crop" 
                 alt="Coaching environment" 
                 class="w-full h-full object-cover object-center brightness-50">
        </div>
        <div class="relative z-10 text-center p-4">
            <h1 class="text-4xl md:text-6xl font-bold text-white mb-4 fade-in-up" style="animation-delay: 0.2s;">엑코치의 코칭 시뮬레이션</h1>
            <p class="text-lg md:text-xl text-gray-300 max-w-2xl mb-8 fade-in-up" style="animation-delay: 0.4s;">
                AI와 함께하는 실전 같은 코칭 연습을 통해 당신의 리더십 잠재력을 깨워보세요.<br>
                다양한 유형의 팀원을 만나고, 실시간 피드백을 받으며 최고의 코치로 성장할 수 있습니다.
            </p>
            <button onclick="enterApp()" class="bg-indigo-600 text-white font-bold py-3 px-10 rounded-lg hover:bg-indigo-700 transition-transform hover:scale-105 text-lg shadow-lg shadow-indigo-500/50 fade-in-up" style="animation-delay: 0.6s;">
                입장하기
            </button>
        </div>
    </div>

    <!-- Main App Container -->
    <div id="main-app" class="container mx-auto p-4 md:p-8 max-w-7xl hidden">
        
        <!-- 헤더 및 탭 -->
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-bold text-white">AI를 활용한 리더의 코칭 시뮬레이션</h1>
            <p class="text-gray-400 mt-2">AI와 함께 코칭 역량을 강화하세요.</p>
        </header>

        <div class="mb-6">
            <div class="flex flex-wrap justify-center border-b border-gray-700">
                <button id="tab-btn-simulation" onclick="showTab('simulation')" class="tab-btn tab-active py-2 px-6 font-semibold rounded-t-lg transition-colors duration-300">AI 시뮬레이션</button>
                <button id="tab-btn-practice" onclick="showTab('practice')" class="tab-btn tab-inactive py-2 px-6 font-semibold rounded-t-lg transition-colors duration-300">실전 코칭 연습</button>
                <button id="tab-btn-analysis" onclick="showTab('analysis')" class="tab-btn tab-inactive py-2 px-6 font-semibold rounded-t-lg transition-colors duration-300">엑코치의 음성분석</button>
                <button id="tab-btn-library" onclick="showTab('library')" class="tab-btn tab-inactive py-2 px-6 font-semibold rounded-t-lg transition-colors duration-300">학습 라이브러리</button>
            </div>
        </div>
        
        <main>
            <!-- 기능 1: AI 시뮬레이션 -->
            <div id="tab-content-simulation" class="tab-content">
                <h2 class="text-2xl font-semibold text-white mb-4">가상 시뮬레이션</h2>
                <p class="text-gray-400 mb-6">AMO 모델 기반 8가지 유형의 가상 팀원과 대화하며 코칭 스킬을 연습해보세요.</p>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                    
                    <!-- 시나리오 카드들 -->
                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">삼무(三無)형</h3><p class="text-sm text-gray-400 mb-3">'방향을 잃은 신입'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-down">A ↓</span><span class="amo-tag amo-down">M ↓</span><span class="amo-tag amo-down">O ↓</span></div>
                            <p class="text-gray-300 text-sm mb-4">신규 입사자로, 업무 이해도와 동기가 모두 낮아 어디서부터 시작해야 할지 몰라 헤매고 있습니다.</p>
                        </div>
                        <button onclick="openChatModal('삼무(三無)형', '방향을 잃은 신입')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">게으른 바보형</h3><p class="text-sm text-gray-400 mb-3">'성장에 무관심한 팀원'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-down">A ↓</span><span class="amo-tag amo-down">M ↓</span><span class="amo-tag amo-neutral">O -</span></div>
                            <p class="text-gray-300 text-sm mb-4">기본 역량과 업무 의욕이 모두 낮으며, 새로운 것을 배우려 하지 않고 최소한의 일만 처리하려 합니다.</p>
                        </div>
                        <button onclick="openChatModal('게으른 바보형', '성장에 무관심한 팀원')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">아픈 손가락형</h3><p class="text-sm text-gray-400 mb-3">'숨겨진 보석'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-down">A ↓</span><span class="amo-tag amo-up">M ↑</span><span class="amo-tag amo-down">O ↓</span></div>
                            <p class="text-gray-300 text-sm mb-4">의욕은 넘치지만 실무 역량이 부족하고, 비효율적인 환경으로 인해 리더의 도움이 절실한 상태입니다.</p>
                        </div>
                        <button onclick="openChatModal('아픈 손가락형', '숨겨진 보석')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">게으른 천재형</h3><p class="text-sm text-gray-400 mb-3">'잠재력을 낭비하는 인재'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-up">A ↑</span><span class="amo-tag amo-down">M ↓</span><span class="amo-tag amo-down">O ↓</span></div>
                            <p class="text-gray-300 text-sm mb-4">역량은 뛰어나지만 현재 업무에 흥미를 잃어, 팀의 비전이나 개인의 성장에 대해 고민하고 있습니다.</p>
                        </div>
                        <button onclick="openChatModal('게으른 천재형', '잠재력을 낭비하는 인재')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">착한 바보형</h3><p class="text-sm text-gray-400 mb-3">'성실한 노력가'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-down">A ↓</span><span class="amo-tag amo-up">M ↑</span><span class="amo-tag amo-up">O ↑</span></div>
                            <p class="text-gray-300 text-sm mb-4">성실하고 책임감이 강하지만, 역량 부족으로 성과가 기대에 미치지 못해 스스로 자책하고 있습니다.</p>
                        </div>
                        <button onclick="openChatModal('착한 바보형', '성실한 노력가')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">워라밸추구형</h3><p class="text-sm text-gray-400 mb-3">'자기 삶을 중시하는 전문가'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-up">A ↑</span><span class="amo-tag amo-down">M ↓</span><span class="amo-tag amo-up">O ↑</span></div>
                            <p class="text-gray-300 text-sm mb-4">높은 역량을 가졌지만 개인의 삶을 중시하여, 추가적인 노력이나 업무 확장을 원치 않습니다.</p>
                        </div>
                        <button onclick="openChatModal('워라밸추구형', '자기 삶을 중시하는 전문가')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">부지런한 천재형</h3><p class="text-sm text-gray-400 mb-3">'좌절에 빠진 에이스'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-up">A ↑</span><span class="amo-tag amo-up">M ↑</span><span class="amo-tag amo-down">O ↓</span></div>
                            <p class="text-gray-300 text-sm mb-4">역량과 동기 모두 뛰어나지만, 비효율적인 환경으로 인해 성과를 내지 못해 강한 좌절감을 느낍니다.</p>
                        </div>
                        <button onclick="openChatModal('부지런한 천재형', '좌절에 빠진 에이스')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>

                    <div class="bg-gray-800 rounded-lg p-5 flex flex-col justify-between hover:bg-gray-700 transition-all duration-300 transform hover:-translate-y-1">
                        <div>
                            <h3 class="font-bold text-lg text-indigo-400 mb-2">만능(萬能)형</h3><p class="text-sm text-gray-400 mb-3">'탁월한 리더 후보'</p>
                            <div class="flex items-center mb-4"><span class="amo-tag amo-up">A ↑</span><span class="amo-tag amo-up">M ↑</span><span class="amo-tag amo-up">O ↑</span></div>
                            <p class="text-gray-300 text-sm mb-4">뛰어난 성과를 내며 완벽히 적응했으며, 새로운 도전과 성장에 대한 강한 욕구를 가지고 있습니다.</p>
                        </div>
                        <button onclick="openChatModal('만능(萬能)형', '탁월한 리더 후보')" class="w-full mt-auto bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors">코칭 시작</button>
                    </div>
                </div>
            </div>
            <!-- 기능 2: 실전 코칭 연습 -->
            <div id="tab-content-practice" class="tab-content hidden">
                 <h2 class="text-2xl font-semibold text-white mb-4">실전 코칭 연습</h2>
                <p class="text-gray-400 mb-6">실제 팀원과의 대화를 앞두고, 팀원의 성향을 설정하여 AI가 생성한 가상 팀원과 대화하며 연습해보세요.</p>
                
                <!-- Step 1: Setup Form -->
                <div id="practice-setup-view">
                    <div class="bg-gray-800 rounded-lg p-8 max-w-3xl mx-auto">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                            <div>
                                <label for="team-member-name-pos" class="block text-gray-300 text-sm font-bold mb-2">팀원 이름 및 직급</label>
                                <input type="text" id="team-member-name-pos" placeholder="예: 김민준 / 마케팅팀 대리" class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-3 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            </div>
                            <div>
                                <label for="conversation-topic" class="block text-gray-300 text-sm font-bold mb-2">코칭 주제</label>
                                <input type="text" id="conversation-topic" placeholder="예: 1분기 성과 리뷰 및 피드백" class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-3 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            </div>
                        </div>
                        <div class="mb-6">
                            <label for="coaching-goal-practice" class="block text-gray-300 text-sm font-bold mb-2">코칭 목표</label>
                            <textarea id="coaching-goal-practice" rows="3" placeholder="예: 이번 대화를 통해 팀원이 자신의 강점을 발견하고, 다음 분기 목표를 스스로 설정하도록 돕는다." class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-3 focus:outline-none focus:ring-2 focus:ring-indigo-500"></textarea>
                        </div>
                         <div class="mb-8">
                            <label for="other-info-practice" class="block text-gray-300 text-sm font-bold mb-2">기타 : 팀원 정보 (최근 팀원 상황 등)</label>
                            <textarea id="other-info-practice" rows="3" placeholder="팀원에 대해 추가적으로 공유하고 싶은 정보가 있다면 입력해주세요. (선택)" class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-3 focus:outline-none focus:ring-2 focus:ring-indigo-500"></textarea>
                        </div>

                        <hr class="border-gray-700 my-8">
                        <h3 class="text-lg font-semibold text-white text-center mb-2">팀원 성향 설정하기</h3>
                        <p class="text-gray-400 text-center mb-8">코칭할 팀원의 성향에 맞게 5단계로 조절해주세요.</p>
                        
                        <!-- Trait Sliders -->
                        <div class="space-y-8">
                            <!-- AMO Model -->
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">역량 (Ability) <span class="text-gray-400 font-normal">- 업무 지식 및 기술 수준</span></label>
                                <div class="trait-scale" id="trait-ability">
                                    <input type="radio" name="ability" value="-2"><input type="radio" name="ability" value="-1"><input type="radio" name="ability" value="0" checked><input type="radio" name="ability" value="1"><input type="radio" name="ability" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>낮음</span><span>높음</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">동기 (Motivation) <span class="text-gray-400 font-normal">- 업무에 대한 열정과 의지</span></label>
                                <div class="trait-scale" id="trait-motivation">
                                    <input type="radio" name="motivation" value="-2"><input type="radio" name="motivation" value="-1"><input type="radio" name="motivation" value="0" checked><input type="radio" name="motivation" value="1"><input type="radio" name="motivation" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>낮음</span><span>높음</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">기회 (Opportunity) <span class="text-gray-400 font-normal">- 성장에 필요한 자원 및 지원 수준</span></label>
                                <div class="trait-scale" id="trait-opportunity">
                                    <input type="radio" name="opportunity" value="-2"><input type="radio" name="opportunity" value="-1"><input type="radio" name="opportunity" value="0" checked><input type="radio" name="opportunity" value="1"><input type="radio" name="opportunity" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>부족</span><span>충분</span></div>
                            </div>
                            <hr class="border-gray-700">
                            <!-- Other Traits -->
                            <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">에너지 방향 <span class="text-gray-400 font-normal">- 주로 어디에서 에너지를 얻나요?</span></label>
                                <div class="trait-scale" id="trait-energy">
                                    <input type="radio" name="energy" value="-2"><input type="radio" name="energy" value="-1"><input type="radio" name="energy" value="0" checked><input type="radio" name="energy" value="1"><input type="radio" name="energy" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>외향적</span><span>내향적</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">판단 기준 <span class="text-gray-400 font-normal">- 무엇을 바탕으로 결정하나요?</span></label>
                                <div class="trait-scale" id="trait-logic">
                                    <input type="radio" name="logic" value="-2"><input type="radio" name="logic" value="-1"><input type="radio" name="logic" value="0" checked><input type="radio" name="logic" value="1"><input type="radio" name="logic" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>논리적</span><span>감성적</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">대화 방식 <span class="text-gray-400 font-normal">- 어떻게 말하는 것을 선호하나요?</span></label>
                                <div class="trait-scale" id="trait-talk">
                                    <input type="radio" name="talk" value="-2"><input type="radio" name="talk" value="-1"><input type="radio" name="talk" value="0" checked><input type="radio" name="talk" value="1"><input type="radio" name="talk" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>단답형</span><span>설명형</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">주장 방식 <span class="text-gray-400 font-normal">- 자신의 의견을 어떻게 표현하나요?</span></label>
                                <div class="trait-scale" id="trait-assertive">
                                    <input type="radio" name="assertive" value="-2"><input type="radio" name="assertive" value="-1"><input type="radio" name="assertive" value="0" checked><input type="radio" name="assertive" value="1"><input type="radio" name="assertive" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>자기주장형</span><span>대세편승형</span></div>
                            </div>
                             <div>
                                <label class="block text-gray-300 text-sm font-bold mb-3">의견 수용 <span class="text-gray-400 font-normal">- 다른 사람의 의견을 어떻게 받아들이나요?</span></label>
                                <div class="trait-scale" id="trait-receptive">
                                    <input type="radio" name="receptive" value="-2"><input type="radio" name="receptive" value="-1"><input type="radio" name="receptive" value="0" checked><input type="radio" name="receptive" value="1"><input type="radio" name="receptive" value="2">
                                </div>
                                <div class="flex items-center justify-between text-xs text-gray-400 px-1 mt-1"><span>의견수용형</span><span>의견배척형</span></div>
                            </div>
                        </div>

                        <button type="button" onclick="generatePracticePersona()" class="w-full mt-10 bg-green-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-green-700 transition-colors">페르소나 생성 및 연습 시작</button>
                    </div>
                </div>

                <!-- Step 2: Persona Summary & Confirmation -->
                <div id="practice-summary-view" class="hidden">
                    <div class="bg-gray-800 rounded-lg p-8 max-w-3xl mx-auto">
                        <h3 class="text-xl font-bold text-white mb-6 text-center">코칭 연습 설정 확인</h3>
                        <div class="bg-gray-700/50 rounded-lg p-5 mb-6 text-left">
                            <h4 class="text-lg font-bold text-green-400 mb-4">입력 정보 요약</h4>
                            <div id="practice-summary-text" class="space-y-3 text-sm"></div>
                        </div>
                        <div class="bg-gray-700/50 rounded-lg p-5 mb-6 text-left">
                            <h4 class="text-lg font-bold text-green-400 mb-4">AI가 생성한 팀원 페르소나</h4>
                            <div id="generated-persona-container" class="text-sm text-gray-300"></div>
                        </div>
                        <div class="flex gap-4 mt-8">
                            <button onclick="returnToPracticeSetup()" class="w-full bg-gray-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-gray-700 transition-colors">다시 설정하기</button>
                            <button onclick="startPracticeConversation()" class="w-full bg-green-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-green-700 transition-colors">이 내용으로 코칭 시작하기</button>
                        </div>
                    </div>
                </div>
            </div>
            <!-- 기능 3: 엑코치의 음성분석 -->
            <div id="tab-content-analysis" class="tab-content hidden">
                 <h2 class="text-2xl font-semibold text-white mb-4">엑코치의 음성분석</h2>
                <p class="text-gray-400 mb-6">실제 코칭 녹음 파일을 업로드하여 AI 코치로부터 객관적인 피드백을 받아보세요.</p>
                <div class="bg-gray-800 rounded-lg p-8 max-w-2xl mx-auto text-center">
                    <div id="audio-upload-view">
                        <label for="audio-upload" class="cursor-pointer border-2 border-dashed border-gray-600 rounded-lg p-12 flex flex-col items-center justify-center hover:border-indigo-500 transition-colors">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-16 h-16 text-gray-500 mb-4"><path stroke-linecap="round" stroke-linejoin="round" d="M12 18.75a6 6 0 0 0 6-6v-1.5m-6 7.5a6 6 0 0 1-6-6v-1.5m12 4.5-1.46-1.823a.75.75 0 0 0-1.214 0l-1.46 1.823m-4.286 0L6.273 9.677a.75.75 0 0 0-1.214 0L3.591 12.5m12 4.5a.75.75 0 0 0 0-1.5h-.75a.75.75 0 0 0 0 1.5h.75Zm-6 0a.75.75 0 0 0 0-1.5h-.75a.75.75 0 0 0 0 1.5h.75Z" /></svg>
                            <p class="text-lg font-semibold text-gray-300">녹음 파일을 여기에 끌어다 놓거나 클릭하여 업로드하세요</p>
                            <p class="text-sm text-gray-500 mt-2">(MP3, WAV, M4A 등 오디오 파일)</p>
                        </label>
                        <input type="file" id="audio-upload" class="hidden" accept="audio/*" onchange="handleAudioUpload(this.files)">
                        <p id="audio-file-name" class="mt-4 text-gray-400"></p>
                    </div>
                </div>
            </div>
            <!-- 기능 4: 학습 라이브러리 -->
            <div id="tab-content-library" class="tab-content hidden">
                <h2 class="text-2xl font-semibold text-white mb-4">학습 라이브러리</h2>
                <p class="text-gray-400 mb-6">코칭에 도움이 되는 다양한 팁을 확인해보세요.</p>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                    <!-- 1. GROW -->
                    <div onclick="openLibraryDetailModal('grow')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/a78bfa?text=GROW" alt="GROW 모델" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-indigo-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 모델</span>
                            <h3 class="font-bold text-lg text-white mb-2">GROW 모델</h3>
                            <p class="text-gray-400 text-sm">목표(Goal)-현실(Reality)-선택지(Options)-의지(Will) 4단계에 따라 명확한 목표 설정과 실행 계획 수립을 돕는 가장 대표적인 코칭 모델입니다.</p>
                        </div>
                    </div>
                    <!-- 2. SBI -->
                    <div onclick="openLibraryDetailModal('sbi')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/f472b6?text=SBI" alt="SBI 모델" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-pink-500 text-white py-1 px-2 rounded-full mb-2 inline-block">피드백 기술</span>
                            <h3 class="font-bold text-lg text-white mb-2">SBI 피드백 모델</h3>
                            <p class="text-gray-400 text-sm">상황(Situation)-행동(Behavior)-영향(Impact)을 기반으로, 감정적인 비난 대신 객관적이고 구체적인 피드백을 전달하는 효과적인 대화법입니다.</p>
                        </div>
                    </div>
                    <!-- 3. OSKAR -->
                    <div onclick="openLibraryDetailModal('oskar')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/10b981?text=OSKAR" alt="OSKAR 모델" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-emerald-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 모델</span>
                            <h3 class="font-bold text-lg text-white mb-2">OSKAR 모델</h3>
                            <p class="text-gray-400 text-sm">문제보다 해결책과 강점에 집중하는 해결중심 코칭 모델입니다. 긍정적인 변화를 이끌어내고 팀원의 자신감을 높이는 데 효과적입니다.</p>
                        </div>
                    </div>
                    <!-- 4. 비폭력대화 -->
                    <div onclick="openLibraryDetailModal('nvc')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/3b82f6?text=NVC" alt="비폭력대화" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-blue-500 text-white py-1 px-2 rounded-full mb-2 inline-block">소통 방식</span>
                            <h3 class="font-bold text-lg text-white mb-2">비폭력대화 (NVC)</h3>
                            <p class="text-gray-400 text-sm">관찰-느낌-욕구-부탁의 4단계로, 갈등 상황에서 상대방을 비난하지 않고 서로의 필요를 이해하며 공감적인 관계를 형성하는 대화법입니다.</p>
                        </div>
                    </div>
                    <!-- 5. FUEL -->
                    <div onclick="openLibraryDetailModal('fuel')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/f97316?text=FUEL" alt="FUEL 모델" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-orange-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 모델</span>
                            <h3 class="font-bold text-lg text-white mb-2">FUEL 모델</h3>
                            <p class="text-gray-400 text-sm">대화의 틀(Frame)-이해(Understand)-탐색(Explore)-계획(Lay out)의 4단계로 구성된 체계적인 코칭 대화 모델로, 명확한 방향성을 제공합니다.</p>
                        </div>
                    </div>
                    <!-- 6. CLEAR -->
                    <div onclick="openLibraryDetailModal('clear')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/6366f1?text=CLEAR" alt="CLEAR 모델" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-indigo-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 모델</span>
                            <h3 class="font-bold text-lg text-white mb-2">CLEAR 모델</h3>
                            <p class="text-gray-400 text-sm">계약(Contract)-경청(Listen)-탐색(Explore)-행동(Action)-검토(Review) 5단계로, 코칭의 전 과정을 명확히 하고 파트너십을 강조하는 모델입니다.</p>
                        </div>
                    </div>
                    <!-- 7. 경청 -->
                    <div onclick="openLibraryDetailModal('listening')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/84cc16?text=Listening" alt="경청" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-lime-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 스킬</span>
                            <h3 class="font-bold text-lg text-white mb-2">마음을 여는 경청</h3>
                            <p class="text-gray-400 text-sm">단순히 듣는 것을 넘어 상대방의 감정과 의도까지 파악하는 적극적 경청은 모든 코칭의 시작이자 가장 중요한 핵심 기술입니다.</p>
                        </div>
                    </div>
                    <!-- 8. 질문 -->
                    <div onclick="openLibraryDetailModal('questioning')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/eab308?text=Questioning" alt="질문" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-yellow-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 스킬</span>
                            <h3 class="font-bold text-lg text-white mb-2">생각을 여는 질문</h3>
                            <p class="text-gray-400 text-sm">정답을 주는 대신, 스스로 답을 찾도록 돕는 '열린 질문'은 팀원의 잠재력을 이끌어내는 가장 강력한 도구입니다.</p>
                        </div>
                    </div>
                    <!-- 9. 인정과 지지 -->
                    <div onclick="openLibraryDetailModal('support')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/ec4899?text=Support" alt="인정과 지지" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-pink-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 스킬</span>
                            <h3 class="font-bold text-lg text-white mb-2">인정과 지지</h3>
                            <p class="text-gray-400 text-sm">결과뿐만 아니라 과정과 노력을 구체적으로 인정하고 지지해주는 것은 팀원이 어려움을 극복하고 나아가게 하는 원동력이 됩니다.</p>
                        </div>
                    </div>
                    <!-- 10. 신뢰(라포) 형성 -->
                    <div onclick="openLibraryDetailModal('rapport')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/0891b2?text=Rapport" alt="신뢰 형성" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-cyan-600 text-white py-1 px-2 rounded-full mb-2 inline-block">기본 자세</span>
                            <h3 class="font-bold text-lg text-white mb-2">신뢰(라포) 형성</h3>
                            <p class="text-gray-400 text-sm">코칭은 신뢰 관계에서 시작됩니다. 진정성 있는 태도와 공감적 이해를 통해 팀원이 마음을 열 수 있는 안전한 환경을 만드세요.</p>
                        </div>
                    </div>
                    <!-- 11. SMART 목표 설정 -->
                    <div onclick="openLibraryDetailModal('smart')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/ef4444?text=SMART" alt="SMART 목표" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-red-500 text-white py-1 px-2 rounded-full mb-2 inline-block">실용 팁</span>
                            <h3 class="font-bold text-lg text-white mb-2">SMART 목표 설정</h3>
                            <p class="text-gray-400 text-sm">구체적이고(Specific), 측정 가능하며(Measurable), 달성 가능하고(Achievable), 관련 있으며(Relevant), 기한이 정해진(Time-bound) 목표 설정법입니다.</p>
                        </div>
                    </div>
                    <!-- 12. 행동 촉진 -->
                    <div onclick="openLibraryDetailModal('action')" class="bg-gray-800 rounded-lg overflow-hidden hover:shadow-lg transition-shadow cursor-pointer">
                        <img src="https://placehold.co/600x400/1f2937/6b7280?text=Action" alt="행동 촉진" class="w-full h-40 object-cover">
                        <div class="p-5">
                            <span class="text-xs font-semibold bg-gray-500 text-white py-1 px-2 rounded-full mb-2 inline-block">핵심 스킬</span>
                            <h3 class="font-bold text-lg text-white mb-2">행동 촉진</h3>
                            <p class="text-gray-400 text-sm">대화가 단순한 논의로 끝나지 않도록, 구체적인 첫 단계를 설정하고 스스로 약속하게 함으로써 실제 행동으로 이어지도록 돕습니다.</p>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- 채팅 모달 -->
    <div id="chat-modal-backdrop" class="modal-backdrop fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center hidden z-40 opacity-0">
        <div id="chat-modal-container" class="modal-container bg-gray-800 rounded-2xl shadow-2xl w-full max-w-6xl h-[90vh] max-h-[800px] flex flex-col transform scale-95 opacity-0">
            <!-- 모달 헤더 -->
            <div class="flex items-center justify-between p-4 border-b border-gray-700 flex-shrink-0">
                <div class="flex items-center gap-4">
                    <div>
                        <h2 id="chat-modal-title" class="text-xl font-bold text-white"></h2>
                        <p id="chat-modal-subtitle" class="text-sm text-gray-400"></p>
                    </div>
                    <!-- 코칭 정보 확인 툴팁 -->
                    <div class="relative group">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-gray-400 cursor-pointer"><path stroke-linecap="round" stroke-linejoin="round" d="m11.25 11.25.041-.02a.75.75 0 0 1 1.063.852l-.708 2.836a.75.75 0 0 0 1.063.853l.041-.021M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9-3.75h.008v.008H12V8.25Z" /></svg>
                        <div id="chat-info-tooltip" class="tooltip-text absolute top-full mt-2 w-72 p-4 bg-gray-700 text-gray-200 text-sm rounded-lg shadow-lg opacity-0 group-hover:opacity-100 pointer-events-none z-10">
                            <!-- JS로 동적 생성 -->
                        </div>
                    </div>
                </div>
                <div class="flex items-center gap-4">
                    <button onclick="endConversation()" class="bg-red-600 text-white font-semibold py-1 px-4 rounded-lg hover:bg-red-700 transition-colors text-sm">대화종료</button>
                    <button onclick="closeChatModal()" class="text-gray-400 hover:text-white transition-colors">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
            </div>
            
            <!-- 모달 바디 -->
            <div class="flex-1 flex flex-col md:flex-row overflow-hidden">
                <!-- 대화법 추천 및 페르소나 섹션 -->
                <div id="chat-recommendations" class="w-full p-6 flex flex-col justify-center text-center overflow-y-auto">
                    <div id="persona-info" class="bg-gray-700/50 rounded-lg p-5 mb-6 text-left">
                        <h4 class="text-lg font-bold text-indigo-300 mb-4">팀원 페르소나</h4>
                        <div class="space-y-3 text-sm">
                            <p><strong class="font-semibold text-gray-300 w-20 inline-block">이름/직책:</strong> <span id="persona-name-role"></span></p>
                            <p><strong class="font-semibold text-gray-300 w-20 inline-block">특징:</strong> <span id="persona-characteristics" class="text-gray-400"></span></p>
                            <p><strong class="font-semibold text-gray-300 w-20 inline-block">최근 상황:</strong> <span id="persona-situation" class="text-gray-400"></span></p>
                        </div>
                    </div>
                    
                    <!-- 코칭 목표 섹션 -->
                    <div id="coaching-goal-info" class="border-2 border-dashed border-indigo-500/50 rounded-lg p-5 mb-6 text-left">
                         <h4 class="text-lg font-bold text-indigo-300 mb-3 flex items-center gap-2">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6"><path stroke-linecap="round" stroke-linejoin="round" d="M3.75 3v11.25A2.25 2.25 0 0 0 6 16.5h2.25M3.75 3h-1.5m1.5 0h16.5m0 0h1.5m-1.5 0v11.25A2.25 2.25 0 0 1 18 16.5h-2.25m-7.5 0h7.5m-7.5 0-1 3m8.5-3 1 3m0 0 .5 1.5m-.5-1.5h-9.5m0 0-.5 1.5M9 11.25v1.5M12 9v3.75m3-6v6" /></svg>
                            이번 코칭의 목표
                        </h4>
                        <p id="coaching-goal-text" class="text-gray-300"></p>
                    </div>

                    <h3 class="text-lg font-semibold text-white mb-4">이런 대화법은 어떠세요?</h3>
                    <p class="text-gray-400 mb-6">코칭 시작 전, 아래 추천 대화법에 마우스를 올려놓고 팁을 확인해보세요.</p>
                    <div id="recommendation-tags" class="flex flex-wrap gap-4 justify-center"></div>
                    <div class="mt-auto pt-8">
                        <button onclick="startConversation()" class="bg-indigo-600 text-white font-bold py-3 px-10 rounded-lg hover:bg-indigo-700 transition-colors text-lg">대화 시작하기</button>
                    </div>
                </div>

                <!-- 대화 및 피드백 인터페이스 (초기에는 숨김) -->
                <div id="chat-and-feedback-interface" class="w-full flex-1 flex flex-col md:flex-row overflow-y-auto md:overflow-hidden">
                    <!-- 채팅 인터페이스 -->
                    <div class="w-full md:w-3/5 flex flex-col border-gray-700 md:border-r">
                        <div id="chat-messages" class="flex-1 p-6 overflow-y-auto space-y-4"></div>
                        <div class="p-4 border-t border-gray-700">
                            <div class="flex items-center gap-3">
                                <input type="text" id="chat-input" placeholder="메시지를 입력하세요..." class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-4 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                                <!-- 음성 입력 버튼 -->
                                <div class="relative group">
                                    <button id="voice-toggle-btn" onclick="toggleVoiceInput()" class="p-2 rounded-full hover:bg-gray-700 transition-colors">
                                        <svg id="mic-icon" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-gray-400"><path stroke-linecap="round" stroke-linejoin="round" d="M12 18.75a6 6 0 0 0 6-6v-1.5m-6 7.5a6 6 0 0 1-6-6v-1.5m12 5.25v-1.5a6 6 0 0 0-12 0v1.5m12 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" /></svg>
                                    </button>
                                    <div class="tooltip-text absolute bottom-full mb-2 w-48 p-2 bg-gray-700 text-gray-200 text-xs text-center rounded-lg shadow-lg opacity-0 group-hover:opacity-100 pointer-events-none z-10 transform -translate-x-1/2 left-1/2">
                                        음성으로 입력하기
                                    </div>
                                </div>
                                <button id="send-button" class="bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700 transition-colors w-20 h-10 flex items-center justify-center">전송</button>
                            </div>
                        </div>
                    </div>
                    <!-- 실시간 코칭 피드백 패널 -->
                    <div id="coaching-feedback-panel" class="w-full md:w-2/5 p-6 bg-gray-800/50 overflow-y-auto">
                        <div class="flex items-center justify-between mb-4 sticky top-0 bg-gray-800/50 py-2">
                            <h3 class="text-lg font-bold text-indigo-300">실시간 코칭 피드백</h3>
                            <div class="flex items-center space-x-2">
                                <span class="text-xs text-gray-400">OFF</span>
                                <div class="relative inline-block w-10 mr-2 align-middle select-none transition duration-200 ease-in">
                                    <input type="checkbox" name="toggle" id="feedback-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer" checked onchange="toggleFeedback(this.checked)"/>
                                    <label for="feedback-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                </div>
                                <span class="text-xs text-indigo-400">ON</span>
                            </div>
                        </div>
                        <div id="feedback-content" class="space-y-6">
                            <div class="text-center text-gray-500 pt-10"><p>대화를 시작하면<br>이곳에 AI의 피드백이 표시됩니다.</p></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- 코칭 지수 및 최종 피드백 모달 -->
    <div id="summary-modal-backdrop" class="modal-backdrop fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center hidden z-50 opacity-0">
        <div id="summary-modal-container" class="modal-container bg-gray-800 rounded-2xl shadow-2xl w-full max-w-4xl p-8 transform scale-95 opacity-0">
            <!-- JS로 동적 생성 -->
        </div>
    </div>

    <!-- 학습 라이브러리 상세 내용 모달 -->
    <div id="library-detail-backdrop" class="modal-backdrop fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center hidden z-50 opacity-0">
        <div id="library-detail-container" class="modal-container bg-gray-800 rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] flex flex-col transform scale-95 opacity-0">
            <!-- 모달 헤더 -->
            <div class="flex items-center justify-between p-5 border-b border-gray-700 flex-shrink-0">
                <div id="library-detail-header" class="flex items-center gap-4">
                    <!-- JS로 동적 생성 -->
                </div>
                <button onclick="closeLibraryDetailModal()" class="text-gray-400 hover:text-white transition-colors">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                </button>
            </div>
            <!-- 모달 바디 -->
            <div id="library-detail-body" class="p-8 overflow-y-auto">
                <!-- JS로 동적 생성 -->
            </div>
        </div>
    </div>


    <script>
        // ===================================================================================
        // SCRIPT REFACTORED FOR STABILITY AND MAINTAINABILITY
        // ===================================================================================

        // -----------------------------------------------------------------------------------
        // 1. CONFIGURATION & STATE MANAGEMENT (설정 및 상태 관리)
        // -----------------------------------------------------------------------------------

        // API Configuration
        const GEMINI_API_KEY = "AIzaSyBeFX0fdqqco0QcTJUwnB9QlfQgQHhRUIk";

        // Application State
        let activeTab = 'simulation';
        let currentMode = 'simulation'; // 'simulation' or 'practice'
        let currentCoachingType = '';
        let conversationHistory = [];
        let coachingChart = null;
        let practiceContext = {}; // Stores all data for a 'practice' session
        let recognition; // Speech recognition instance
        let isRecording = false;
        let chartImageData = null; // For saving chart image to DOCX
        let isFeedbackEnabled = true;
        let finalReportData = null; // For saving final report data to DOCX

        // DOM Elements (will be initialized in DOMContentLoaded)
        let ui = {};

        // -----------------------------------------------------------------------------------
        // 2. STATIC DATA (정적 데이터)
        // -----------------------------------------------------------------------------------

        const personas = {
            '삼무(三無)형': { name: "김초롱", role: "마케팅팀 / 신입 (3개월)", characteristics: "의욕은 있으나, 무엇을 해야 할지 몰라 자주 멍하니 있거나 동료들에게 기본적인 것을 계속 물어봐서 눈치를 보고 있음.", situation: "최근 담당한 소셜 미디어 채널 관리 업무에서, 경쟁사 분석 보고서 제출을 계속 누락하고 포스팅에 오타가 잦아 팀장에게 여러 번 지적을 받았습니다. 이후 눈에 띄게 위축되어 동료들에게 질문하는 것조차 망설이는 모습을 보입니다.", coachingGoal: "팀원이 현재 겪는 어려움의 원인을 스스로 파악하고, 자신감을 회복하여 구체적인 실행 계획(Action Item)을 1가지 이상 설정하도록 돕는다." },
            '게으른 바보형': { name: "박대충", role: "운영팀 / 5년차", characteristics: "딱 시키는 일만, 마감 기한에 맞춰 최소한으로 처리함. 새로운 업무나 스터디 제안에 '굳이 그걸 왜...'라는 반응을 보임.", situation: "팀에서 새로 도입하는 자동화 툴 교육에 '바쁘다'는 핑계로 불참했습니다. 이후 동료들이 새 툴로 업무를 처리할 때 혼자 기존 방식을 고수하며 \"이게 더 편하다\"고 말해 팀워크를 해치고 있으며, 이로 인해 데이터 취합 과정에서 병목 현상이 발생하고 있습니다.", coachingGoal: "새로운 툴 도입에 대한 팀원의 저항감의 원인을 파악하고, 변화의 필요성을 스스로 인식하여 교육에 참여하도록 유도한다." },
            '아픈 손가락형': { name: "이열정", role: "기획팀 / 2년차", characteristics: "아이디어가 넘치고 야근도 마다하지 않지만, 결과물의 완성도가 떨어지고 방향을 자주 잃음. '제가 더 열심히 하겠습니다!'를 입에 달고 삼.", situation: "야심차게 준비한 신규 서비스 제안서가 '핵심 타겟 고객이 불분명하다'는 이유로 반려되었습니다. 이후 밤을 새워 수정안을 만들었지만 또다시 같은 지적을 받자, \"전 역시 기획엔 재능이 없나 봐요\"라며 자책하며 거의 손을 놓고 있는 상태입니다.", coachingGoal: "팀원의 노력을 인정하고 자신감을 회복시켜주며, 문제의 원인을 외부가 아닌 자신의 역량 부족에서 찾도록 유도하여 구체적인 역량 강화 계획을 세우도록 돕는다." },
            '게으른 천재형': { name: "최시크", role: "개발팀 / 7년차 (시니어)", characteristics: "복잡한 기술 문제를 누구보다 빠르고 정확하게 해결하지만, 단순 반복적인 업무에는 노골적으로 불만을 표하며 동기 부여가 전혀 되지 않음.", situation: "팀의 핵심 기술 부채를 해결하는 데 큰 기여를 한 후, 간단한 유지보수 업무만 주어지자 회의 시간에 노골적으로 하품을 하거나 스마트폰을 보는 등 불성실한 태도를 보입니다. 최근에는 \"더 성장할 수 있는 곳으로 가고 싶다\"며 이직을 암시하는 발언을 했습니다.", coachingGoal: "팀원의 역량과 기여를 인정해주고, 현재 업무에 대한 불만의 진짜 원인을 파악하여 팀의 목표와 개인의 성장 목표를 연결할 수 있는 새로운 도전 과제를 함께 찾아본다." },
            '착한 바보형': { name: "정성실", role: "디자인팀 / 4년차", characteristics: "누구보다 꼼꼼하고 성실하며, 동료의 부탁도 거절하지 못함. 하지만 디자인 트렌드에 뒤쳐지고 결과물의 창의성이 부족하다는 평을 받음.", situation: "중요한 프로젝트의 메인 시안이 '너무 올드하다'는 이유로 채택되지 못했습니다. 이후 밤을 새워 수십 개의 시안을 만들었지만, 피드백을 반영하지 않고 양으로만 승부하려 해 동료들을 지치게 만들고 있습니다. 현재 스트레스가 극에 달한 상태입니다.", coachingGoal: "팀원의 성실함과 노력을 인정해주고, 업무 방식의 비효율성을 스스로 깨닫게 하여 '어떻게' 일할 것인지에 대한 구체적인 개선 계획을 세우도록 돕는다." },
            '워라밸추구형': { name: "오칼퇴", role: "재무팀 / 6년차", characteristics: "정해진 업무 시간에는 최고의 효율과 집중력을 보여주지만, 6시가 되면 1초의 망설임도 없이 퇴근함. 업무 외적인 팀 활동에는 거의 참여하지 않음.", situation: "분기 마감으로 팀 전체가 야근하는 상황에서, 자신의 업무가 끝났다며 \"먼저 들어가겠습니다\"라고 말하고 정시 퇴근했습니다. 이로 인해 다른 팀원들이 그의 남은 일을 처리해야 했고, 팀 내에서 \"개인주의적이다\"라는 불만이 나오고 있습니다.", coachingGoal: "팀원의 업무 스타일을 존중하면서도, 팀워크의 중요성을 스스로 인식하게 하여 팀 목표 달성을 위한 최소한의 협력 방안을 함께 모색한다." },
            '부지런한 천재형': { name: "나에이스", role: "사업개발팀 / 3년차", characteristics: "뛰어난 역량과 열정으로 항상 새로운 프로젝트를 주도하지만, 회사의 복잡한 보고 절차와 부서 간의 비협조적인 태도 때문에 번번이 좌절을 겪음.", situation: "중요한 파트너십 계약이 타 부서의 비협조와 복잡한 보고 절차 때문에 무산될 위기에 처하자, 팀 미팅에서 \"이런 식으로는 아무것도 할 수 없다\"며 목소리를 높였습니다. 현재 강한 무력감과 분노를 느끼며 프로젝트 진행을 거의 포기한 상태입니다.", coachingGoal: "팀원의 좌절감에 깊이 공감해주고, 현재 상황을 긍정적으로 재해석하여 통제 가능한 영역에 집중하도록 유도함으로써 문제 해결의 주도성을 되찾게 한다." },
            '만능(萬能)형': { name: "왕고수", role: "데이터분석팀 / 8년차 (파트장)", characteristics: "현재 맡은 업무는 완벽하게 수행하며 팀원들의 멘토 역할까지 훌륭히 해내고 있음. 현재에 안주하지 않고 새로운 도전과 성장을 갈망함.", situation: "최근 팀의 모든 목표를 초과 달성하고, 주니어 팀원들의 멘토 역할까지 훌륭히 수행했습니다. 만족감보다는 새로운 도전에 대한 갈증을 느끼고 있으며, \"다음 스텝으로 제가 무엇을 더 기여하고 성장할 수 있을까요?\"라며 리더에게 면담을 요청한 상황입니다.", coachingGoal: "팀원의 성과와 성장에 대한 열정을 인정하고, 장기적인 커리어 목표를 함께 논의하여 현재 팀의 목표와 연계된 새로운 도전 과제나 역할을 부여한다." }
        };
        const conversationMethods = { GROW: { name: "GROW", description: "Goal(목표), Reality(현실), Options(선택지), Will(실행의지)의 4단계로 구성된 코칭 모델입니다. 목표를 명확히 하고 달성 계획을 세우는 데 효과적입니다." }, OSKAR: { name: "OSKAR", description: "Outcome(결과), Scaling(척도), Know-how(노하우), Affirm+Action(지지+실행), Review(검토)에 초점을 맞춘 해결중심 코칭 모델입니다. 긍정적 변화에 집중합니다." }, CLEAR: { name: "CLEAR", description: "Contracting(계약), Listening(경청), Exploring(탐색), Action(실행), Review(검토)의 과정을 통해 코칭 대화를 구조화합니다. 코칭의 전 과정을 명확하게 합니다." }, '4A': { name: "4A 피드백", description: "Aim(의도 확인), Ask(상황 질문), Acknowledge(인정/공감), Action(행동 계획)의 4단계로 구성된 피드백 모델입니다. 건설적인 피드백을 제공하는 데 유용합니다." }, NVC: { name: "비폭력대화 (NVC)", description: "Observation(관찰), Feeling(느낌), Needs(욕구), Request(부탁)의 4가지 요소로 구성됩니다. 갈등 상황에서 공감대를 형성하고 해결책을 찾는 데 도움을 줍니다." }, FUEL: { name: "FUEL", description: "Frame(대화의 틀 설정), Understand(현재 상태 이해), Explore(원하는 상태 탐색), Lay out(성공 계획 수립)의 4단계로 구성된 코칭 대화 모델입니다." } };
        const recommendations = { '삼무(三無)형': ['GROW', 'FUEL', 'CLEAR'], '게으른 바보형': ['4A', 'FUEL', 'NVC'], '아픈 손가락형': ['GROW', 'OSKAR', '4A'], '게으른 천재형': ['GROW', 'FUEL'], '착한 바보형': ['OSKAR', '4A', 'GROW'], '워라밸추구형': ['GROW', 'OSKAR'], '부지런한 천재형': ['CLEAR', 'FUEL', 'OSKAR'], '만능(萬能)형': ['GROW', 'CLEAR'] };

        // -----------------------------------------------------------------------------------
        // 3. API COMMUNICATION (API 통신)
        // -----------------------------------------------------------------------------------

        /**
         * A centralized function to call the Gemini API.
         * @param {object} payload - The payload to send to the API.
         * @returns {Promise<string>} The text response from the API.
         * @throws Will throw an error if the API call fails.
         */
        async function callGeminiApi(payload) {
            const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${GEMINI_API_KEY}`;
            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    const errorBody = await response.text();
                    console.error(`API Error: ${response.status}`, errorBody);
                    throw new Error(`API request failed with status ${response.status}`);
                }
                
                const result = await response.json();

                if (!result.candidates || result.candidates.length === 0 || !result.candidates[0].content || !result.candidates[0].content.parts) {
                    console.error("API Error: Invalid response structure", result);
                    throw new Error("Invalid response from API: No candidates or content.");
                }

                return result.candidates[0].content.parts[0].text;

            } catch (error) {
                console.error("Error in callGeminiApi:", error);
                throw error; // Re-throw the error to be handled by the calling function
            }
        }

        // -----------------------------------------------------------------------------------
        // 4. CORE LOGIC & CONTROLLERS (핵심 로직 및 컨트롤러)
        // -----------------------------------------------------------------------------------

        /**
         * Handles sending a user message and receiving AI response and feedback.
         */
        async function handleSendMessage() {
            const messageText = ui.chatInput.value.trim();
            if (!messageText) return;

            addMessage('user', messageText);
            ui.chatInput.value = '';
            setSendButtonLoading(true);
            
            let feedbackPromise = null;
            if (isFeedbackEnabled) {
                ui.feedbackContent.innerHTML = `<div class="text-center text-gray-400 animate-pulse pt-10"><p>피드백을 생성하고 있습니다...</p></div>`;
                feedbackPromise = getCoachingFeedback(messageText);
            } else {
                ui.feedbackContent.innerHTML = '';
            }

            const chatResponsePromise = getAiChatResponse();

            try {
                const [feedbackData, aiResponse] = await Promise.all([feedbackPromise, chatResponsePromise]);
                
                if (isFeedbackEnabled && feedbackData) {
                    updateFeedbackPanel(feedbackData);
                }
                addMessage('ai', aiResponse);
            } catch (error) {
                console.error("Error during message handling:", error);
                addMessage('ai', '죄송합니다, 응답 생성 중 오류가 발생했습니다. 잠시 후 다시 시도해주세요.');
            } finally {
                setSendButtonLoading(false);
            }
        }

        /**
         * Ends the conversation and initiates the summary/report generation.
         */
        async function endConversation() {
            const userMessageCount = conversationHistory.filter(m => m.role === 'user').length;

            showSummaryModal();

            if (userMessageCount < 3) {
                displayAnalysisUnavailable();
                return;
            }
            
            setSummaryModalLoading("AI 코치가 대화 내용을 분석하여<br>최종 리포트를 생성하고 있습니다...");

            try {
                const analysisData = await getCoachingAnalysis();
                if (currentMode === 'practice') {
                    const reportData = await generateFinalReport("성찰 없음");
                    finalReportData = reportData; // Save for download
                    displayPracticeFinalReport(analysisData, reportData);
                } else {
                    displayCoachingIndex(analysisData);
                }
            } catch (error) {
                console.error("Error ending conversation:", error);
                displayAnalysisError();
            }
        }

        /**
         * Generates a persona for the practice mode based on user inputs.
         */
        async function generatePracticePersona() {
            const teamMember = ui.teamMemberNamePosInput.value.trim();
            const topic = ui.conversationTopicInput.value.trim();
            const goal = ui.coachingGoalPracticeInput.value.trim();
            const otherInfo = ui.otherInfoPracticeInput.value.trim();
            
            if (!teamMember || !topic || !goal) {
                alert('팀원 이름/직급, 코칭 주제, 코칭 목표는 필수 입력 항목입니다.');
                return;
            }

            const traits = {
                ability: document.querySelector('input[name="ability"]:checked').value,
                motivation: document.querySelector('input[name="motivation"]:checked').value,
                opportunity: document.querySelector('input[name="opportunity"]:checked').value,
                energy: document.querySelector('input[name="energy"]:checked').value,
                logic: document.querySelector('input[name="logic"]:checked').value,
                talk: document.querySelector('input[name="talk"]:checked').value,
                assertive: document.querySelector('input[name="assertive"]:checked').value,
                receptive: document.querySelector('input[name="receptive"]:checked').value
            };
            
            practiceContext = { teamMember, topic, goal, otherInfo, traits };

            // Switch to summary view
            ui.practiceSetupView.classList.add('hidden');
            ui.practiceSummaryView.classList.remove('hidden');

            displayPracticeSummary();
            const personaContainer = document.getElementById('generated-persona-container');
            personaContainer.innerHTML = `<p class="animate-pulse">AI가 팀원 페르소나를 생성하고 있습니다...</p>`;

            try {
                const personaDescription = await getGeneratedPersona(practiceContext);
                practiceContext.personaDescription = personaDescription;
                personaContainer.innerHTML = `<p>${personaDescription}</p>`;
            } catch (error) {
                personaContainer.innerHTML = `<p class="text-red-400">페르소나 생성 중 오류가 발생했습니다. 다시 시도해주세요.</p>`;
            }
        }
        
        /**
         * Submits the user's reflection to generate the final report.
         * @param {boolean} isSkipped - Whether the user skipped the reflection step.
         */
        async function submitReflection(isSkipped = false) {
            const reflectionInput = document.getElementById('reflection-input');
            const reflectionText = isSkipped ? "사용자가 성찰 내용 입력을 건너뛰었습니다." : (reflectionInput ? reflectionInput.value.trim() : "성찰 내용 없음");
            
            if (!isSkipped && !reflectionText) {
                alert('성찰 내용을 입력하거나 건너뛰기를 선택해주세요.');
                return;
            }

            setSummaryModalLoading("AI 코치가 최종 피드백을 작성하고 있습니다.<br>잠시만 기다려주세요...");
            
            try {
                const reportText = await generateFinalReport(reflectionText);
                finalReportData = { finalFeedback: reportText }; // Save for potential future download feature
                displayFinalReport(reportText);
            } catch (error) {
                console.error("Error submitting reflection:", error);
                displayAnalysisError();
            }
        }

        // -----------------------------------------------------------------------------------
        // 5. API-DEPENDENT LOGIC (API 호출 로직)
        // -----------------------------------------------------------------------------------

        /**
         * Gets AI chat response based on conversation history and persona.
         */
        async function getAiChatResponse() {
            let systemPrompt = '';
            const attitudeModifier = getAttitudeModifier();

            if (currentMode === 'simulation') {
                const persona = personas[currentCoachingType];
                systemPrompt = `당신은 리더십 코칭 시뮬레이션의 가상 팀원입니다. 당신의 이름은 ${persona.name}이고, 역할은 '${currentCoachingType}'(${persona.role})입니다. 당신의 특징은 '${persona.characteristics}'이며, 최근 상황은 '${persona.situation}'입니다. 이 페르소나에 완전히 몰입하여 리더(사용자)의 질문에 응답하세요. 자연스럽고 현실적인 대화를 유지해야 합니다. ${attitudeModifier} **답변은 간결하게, 2-3문장으로 요약해서 말해주세요.**`;
            } else { // practice mode
                const traitsDescription = getTraitsDescription();
                systemPrompt = `당신은 리더십 코칭 시뮬레이션의 가상 팀원입니다. 당신의 이름과 직급은 '${practiceContext.teamMember}'입니다. 당신의 성향은 다음과 같습니다: ${traitsDescription}. AI가 생성한 당신의 페르소나 설명은 다음과 같습니다: "${practiceContext.personaDescription}". 이 페르소나와 성향에 완전히 몰입하여 리더(사용자)의 질문에 응답하세요. 자연스럽고 현실적인 대화를 유지해야 합니다. ${attitudeModifier} **답변은 간결하게, 2-3문장으로 요약해서 말해주세요.**`;
            }
            
            const chatHistoryForApi = [{ role: "user", parts: [{ text: systemPrompt }] }, ...conversationHistory];
            const payload = { contents: chatHistoryForApi };

            try {
                return await callGeminiApi(payload);
            } catch (error) {
                return "죄송합니다. 지금은 답변을 생성할 수 없어요. 잠시 후 다시 시도해주세요.";
            }
        }
        
        /**
         * Gets real-time coaching feedback for the user's last message.
         */
        async function getCoachingFeedback(lastUserMessage) {
            const contextPrompt = getContextPrompt();
            const feedbackPrompt = `
                당신은 리더십 코칭 전문가입니다. 다음 대화를 분석하세요.
                ${contextPrompt}
                **대화 기록:**
                ${JSON.stringify(conversationHistory.slice(-6))}
                **리더의 마지막 메시지:**
                "${lastUserMessage}"
                **요청:** 이 내용을 바탕으로, 리더를 위한 피드백(피드백, 팀원 예상 감정, 추천 질문 3가지)을 JSON 객체 형식으로 제공해주세요.
            `;
            const payload = {
                contents: [{ role: "user", parts: [{ text: feedbackPrompt }] }],
                generationConfig: { 
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            feedback: { "type": "ARRAY", "items": { "type": "STRING" } },
                            teamMemberFeeling: { "type": "ARRAY", "items": { "type": "STRING" } },
                            suggestions: { "type": "ARRAY", "items": { "type": "STRING" } }
                        }
                    }
                }
            };

            try {
                const jsonText = await callGeminiApi(payload);
                return JSON.parse(jsonText);
            } catch (error) {
                return {
                    feedback: ["피드백 생성 중 오류가 발생했습니다."],
                    teamMemberFeeling: ["알 수 없음"],
                    suggestions: ["다시 시도해주세요."]
                };
            }
        }

        /**
         * Gets the overall coaching analysis at the end of the conversation.
         */
        async function getCoachingAnalysis() {
            const contextPrompt = getContextPrompt();
            const analysisPrompt = `
                당신은 마스터 코치입니다. 리더와 팀원 간의 다음 코칭 대화 전체를 분석하여, 리더의 코칭 역량을 5가지 핵심 기준으로 평가해주세요.
                ${contextPrompt}
                **전체 대화 내용:**
                ${JSON.stringify(conversationHistory)}
                **요청:** 대화 내용을 바탕으로, 5가지 코칭 역량(경청, 질문, 공감, 인정/지지, 행동촉진)에 대한 점수(0-100), 강점(1-2개), 개선점(1-2개)을 JSON 형식으로 분석해주세요.
            `;
            const payload = {
                contents: [{ role: "user", parts: [{ text: analysisPrompt }] }],
                generationConfig: { 
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            scores: {
                                type: "OBJECT",
                                properties: {
                                    listening: { "type": "NUMBER" },
                                    questioning: { "type": "NUMBER" },
                                    empathy: { "type": "NUMBER" },
                                    support: { "type": "NUMBER" },
                                    action: { "type": "NUMBER" }
                                }
                            },
                            strengths: { "type": "ARRAY", "items": { "type": "STRING" } },
                            weaknesses: { "type": "ARRAY", "items": { "type": "STRING" } }
                        }
                    }
                }
            };

            try {
                const jsonText = await callGeminiApi(payload);
                return JSON.parse(jsonText);
            } catch (error) {
                return {
                    scores: { listening: 0, questioning: 0, empathy: 0, support: 0, action: 0 },
                    strengths: ["분석 중 오류가 발생했습니다."],
                    weaknesses: ["잠시 후 다시 시도해주세요."]
                };
            }
        }

        /**
         * Generates the final, comprehensive report.
         */
        async function generateFinalReport(reflectionText) {
            let prompt, payload;

            if (currentMode === 'practice') {
                prompt = `
                    당신은 따뜻하고 통찰력 있는 리더십 코칭 전문가입니다.
                    리더가 방금 가상 팀원과의 코칭 시뮬레이션을 마쳤습니다.
                    아래 대화 전체 내용과 리더가 스스로 성찰한 내용을 바탕으로, 리더를 위한 종합적인 최종 리포트를 JSON 형식으로 작성해주세요.

                    **코칭 상황 정보:**
                    - 팀원: ${practiceContext.teamMember}
                    - 코칭 주제: ${practiceContext.topic}
                    - 생성된 페르소나: ${practiceContext.personaDescription}

                    **전체 대화 내용:**
                    ${JSON.stringify(conversationHistory)}

                    **리더의 성찰 내용:**
                    "${reflectionText}"

                    **작성 가이드:**
                    1.  **finalFeedback (최종 피드백):**
                        -   격려와 인정, 강점 분석, 성장 포인트 제안, 긍정적 마무리 메시지를 포함하여 전문가적이면서도 따뜻한 톤으로 작성해주세요.
                    2.  **improvedScript (개선된 코칭 스크립트):**
                        -   이상적인 코칭 대화 스크립트 전체를 리더와 팀원의 대화 형식으로 작성해주세요. (예: 리더: "...", 팀원: "...")
                    - 모든 내용은 반드시 한국어로 작성해야 합니다.
                `;
                payload = {
                    contents: [{ role: "user", parts: [{ text: prompt }] }],
                    generationConfig: {
                        responseMimeType: "application/json",
                        responseSchema: {
                            type: "OBJECT",
                            properties: {
                                finalFeedback: { "type": "STRING" },
                                improvedScript: { "type": "STRING" }
                            }
                        }
                    }
                };
            } else { // simulation
                const persona = personas[currentCoachingType];
                prompt = `
                    당신은 따뜻하고 통찰력 있는 리더십 코칭 전문가입니다.
                    리더가 방금 가상 팀원과의 코칭 시뮬레이션을 마쳤습니다.
                    아래 대화 전체 내용과 리더가 스스로 성찰한 내용을 바탕으로, 리더를 위한 종합적인 최종 피드백을 작성해주세요.

                    **코칭 대상 팀원 정보:**
                    - 이름: ${persona.name}
                    - 유형: ${currentCoachingType}
                    - 특징: ${persona.characteristics}
                    - 최근 상황: ${persona.situation}

                    **전체 대화 내용:**
                    ${JSON.stringify(conversationHistory)}

                    **리더의 성찰 내용:**
                    "${reflectionText}"

                    **피드백 작성 가이드:**
                    1.  **격려와 인정:** 먼저 시뮬레이션을 마친 리더의 노력을 칭찬하고, 성찰 내용에 대해 공감하며 피드백을 시작해주세요.
                    2.  **강점 분석:** 대화 내용과 성찰 내용을 바탕으로 리더가 보여준 코칭적 강점(예: 좋은 질문, 공감적 경청, 해결책 모색 등)을 2~3가지 구체적으로 짚어주세요.
                    3.  **성장 포인트 제안:** 리더가 어려움을 느낀 지점과 대화 내용을 연결하여, 앞으로 시도해볼 수 있는 구체적인 행동 팁이나 다른 관점을 1~2가지 제안해주세요.
                    4.  **마무리:** 전체 내용을 요약하고, 앞으로의 성장을 응원하는 긍정적인 메시지로 마무리해주세요.
                    5.  **스타일:** 전체적으로 전문가적이면서도 따뜻하고, 지시가 아닌 제안의 형태로 작성해주세요. 모든 내용은 반드시 한국어로 작성해야 합니다.
                `;
                payload = { contents: [{ role: "user", parts: [{ text: prompt }] }] };
            }
            
            try {
                const text = await callGeminiApi(payload);
                return currentMode === 'practice' ? JSON.parse(text) : text;
            } catch (error) {
                if (currentMode === 'practice') {
                    return { 
                        finalFeedback: "최종 피드백 생성 중 오류가 발생했습니다.", 
                        improvedScript: "스크립트 생성 중 오류가 발생했습니다." 
                    };
                }
                return "최종 피드백을 생성하는 중 오류가 발생했습니다. 잠시 후 다시 시도해주세요.";
            }
        }

        /**
         * Generates a persona description for the practice mode.
         */
        async function getGeneratedPersona(context) {
            const traitsDescription = getTraitsDescription(true); // Get descriptive trait names
            const prompt = `
                당신은 조직 심리학 전문가입니다. 리더가 코칭을 연습할 가상의 팀원 페르소나를 생성해주세요.
                
                **입력 정보:**
                - 팀원: ${context.teamMember}
                - 코칭 주제: ${context.topic}
                - 코칭 목표: ${context.goal}
                - 추가 정보: ${context.otherInfo || '없음'}
                - 성향:
                    ${traitsDescription}

                **요청:**
                위 정보를 바탕으로, 이 팀원이 어떤 사람일지 구체적인 행동 특성을 포함하여 2~3문장으로 간결하게 묘사해주세요.
                예시: "김민준 대리는 평소에는 조용하지만, 데이터에 기반한 논리적인 근거를 제시할 때 자기주장이 강해지는 경향이 있습니다. 최근 성과 리뷰를 앞두고 자신의 성과가 저평가될까 우려하며 다소 방어적인 태도를 보일 수 있습니다."
                반드시 한국어로 작성해주세요.
            `;
            
            try {
                const payload = { contents: [{ role: "user", parts: [{ text: prompt }] }] };
                return await callGeminiApi(payload);
            } catch (error) {
                throw error; // Let the calling function handle the UI update
            }
        }
        
        // -----------------------------------------------------------------------------------
        // 6. UI & VIEW LOGIC (UI 및 뷰 로직)
        // -----------------------------------------------------------------------------------
        
        function showTab(tabId) {
            ['simulation', 'practice', 'analysis', 'library'].forEach(t => {
                document.getElementById('tab-content-' + t).classList.add('hidden');
                const btn = document.getElementById('tab-btn-' + t);
                btn.classList.remove('tab-active');
                btn.classList.add('tab-inactive');
            });

            document.getElementById('tab-content-' + tabId).classList.remove('hidden');
            const activeBtn = document.getElementById('tab-btn-' + tabId);
            activeBtn.classList.add('tab-active');
            activeBtn.classList.remove('tab-inactive');
            activeTab = tabId;
        }

        function openChatModal(title, subtitle) {
            currentMode = 'simulation';
            currentCoachingType = title;
            resetChatState();

            ui.chatModalTitle.textContent = `${title} 유형 코칭`;
            ui.chatModalSubtitle.textContent = `'${subtitle}'`;

            const persona = personas[title];
            if (persona) {
                ui.personaNameRole.textContent = `${persona.name} / ${persona.role}`;
                ui.personaCharacteristics.textContent = persona.characteristics;
                ui.personaSituation.textContent = persona.situation;
                ui.coachingGoalText.textContent = persona.coachingGoal;
            }

            const recommendedMethods = recommendations[title] || [];
            ui.recommendationTags.innerHTML = recommendedMethods.map(key => {
                const method = conversationMethods[key];
                return `<div class="relative group"><span class="cursor-pointer bg-gray-600 text-gray-200 py-2 px-4 rounded-full transition-colors hover:bg-gray-500">${method.name}</span><div class="tooltip-text absolute bottom-full mb-2 w-72 p-4 bg-gray-700 text-gray-200 text-sm rounded-lg shadow-lg opacity-0 group-hover:opacity-100 pointer-events-none z-10 transform -translate-x-1/2 left-1/2"><h4 class="font-bold text-white mb-2">${method.name}</h4><p class="text-gray-300 text-left">${method.description}</p></div></div>`;
            }).join('');

            ui.chatRecommendations.classList.remove('hidden');
            ui.chatAndFeedbackInterface.classList.add('hidden');
            document.getElementById('coaching-goal-info').classList.remove('hidden');
            
            updateChatInfoTooltip();
            showChatModal();
        }

        function startPracticeConversation() {
            currentMode = 'practice';
            resetChatState();

            ui.chatModalTitle.textContent = '실전 코칭 연습';
            ui.chatModalSubtitle.textContent = `'${practiceContext.topic}'`;
            
            ui.chatRecommendations.classList.add('hidden');
            ui.chatAndFeedbackInterface.classList.remove('hidden');
            ui.chatAndFeedbackInterface.classList.add('md:flex');
            
            const initialMessage = `안녕하세요, ${practiceContext.teamMember.split('/')[0].trim()}입니다. 리더님, 무슨 일로 저를 부르셨나요?`;
            ui.chatMessages.innerHTML = ``;
            addMessage('ai', initialMessage);

            ui.feedbackContent.innerHTML = `<div class="text-center text-gray-500 pt-10"><p>연습할 말을 입력하면<br>이곳에 AI의 피드백이 표시됩니다.</p></div>`;
            
            updateChatInfoTooltip();
            showChatModal();
        }
        
        function startConversation() {
            ui.chatRecommendations.classList.add('hidden');
            ui.chatAndFeedbackInterface.classList.remove('hidden');
            ui.chatAndFeedbackInterface.classList.add('md:flex');
            
            const persona = personas[currentCoachingType];
            const initialMessage = `안녕하세요, ${persona.name}입니다. 리더님, 무슨 일로 저를 부르셨나요? 편하게 말씀해주세요.`;
            
            ui.chatMessages.innerHTML = '';
            addMessage('ai', initialMessage);
            
            ui.feedbackContent.innerHTML = `<div class="text-center text-gray-500 pt-10"><p>대화를 시작하면<br>이곳에 AI의 피드백이 표시됩니다.</p></div>`;
        }

        function addMessage(sender, text) {
            const messageElement = document.createElement('div');
            let content = '';
            if (sender === 'user') {
                content = `<div class="flex items-start gap-3 justify-end"><div class="bg-blue-600 p-3 rounded-lg max-w-lg"><p class="text-sm text-white">${text}</p></div><div class="w-8 h-8 rounded-full bg-blue-600 flex items-center justify-center font-bold text-white text-sm shrink-0">나</div></div>`;
                conversationHistory.push({ role: "user", parts: [{ text }] });
            } else {
                content = `<div class="flex items-start gap-3"><div class="w-8 h-8 rounded-full bg-indigo-500 flex items-center justify-center font-bold text-white text-sm shrink-0">AI</div><div class="bg-gray-700 p-3 rounded-lg max-w-lg"><p class="text-sm text-gray-200">${text}</p></div></div>`;
                conversationHistory.push({ role: "model", parts: [{ text }] });
            }
            messageElement.innerHTML = content;
            ui.chatMessages.appendChild(messageElement);
            ui.chatMessages.scrollTop = ui.chatMessages.scrollHeight;
        }

        function updateFeedbackPanel(feedbackData) {
            const suggestionsHTML = feedbackData.suggestions.map(suggestion => 
                `<button onclick="applySuggestion(this)" class="w-full text-left p-3 text-sm text-gray-300 rounded-lg transition-colors feedback-suggestion-btn">${suggestion}</button>`
            ).join('');

            const feelingTitle = currentMode === 'practice' ? "팀원이 느낄 수 있는 감정" : "AI 팀원의 예상 감정";

            ui.feedbackContent.innerHTML = `
                <div class="space-y-2">
                    <h4 class="font-semibold text-gray-300">방금 한 말에 대한 피드백</h4>
                    <ul class="list-disc list-inside text-sm text-gray-400 bg-gray-700/50 p-3 rounded-md space-y-1">${feedbackData.feedback.map(item => `<li>${item}</li>`).join('')}</ul>
                </div>
                <div class="space-y-2">
                    <h4 class="font-semibold text-gray-300">${feelingTitle}</h4>
                    <ul class="list-disc list-inside text-sm text-gray-400 bg-gray-700/50 p-3 rounded-md space-y-1">${feedbackData.teamMemberFeeling.map(item => `<li>${item}</li>`).join('')}</ul>
                </div>
                <div class="space-y-2">
                    <h4 class="font-semibold text-gray-300">이렇게 말해보세요 (추천 질문)</h4>
                    <div class="space-y-2">${suggestionsHTML}</div>
                </div>
            `;
        }

        function displayCoachingIndex(data) {
            const scores = Object.values(data.scores);
            const totalScore = Math.round(scores.reduce((a, b) => a + b, 0) / scores.length);

            ui.summaryModalContainer.innerHTML = `
                <div class="flex items-center justify-between mb-6">
                    <h3 class="text-2xl font-bold text-white">1단계: 나의 코칭 지수 확인</h3>
                    <button onclick="closeSummaryModal()" class="text-gray-400 hover:text-white transition-colors">
                        <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center mb-8">
                    <div><canvas id="coaching-chart"></canvas></div>
                    <div class="space-y-6">
                        <div class="text-center bg-gray-700/50 p-4 rounded-lg">
                            <p class="text-gray-400 text-sm">총점</p>
                            <p class="text-4xl font-bold text-indigo-400">${totalScore}<span class="text-2xl text-gray-400">점</span></p>
                        </div>
                        <div>
                            <h4 class="font-semibold text-green-400 mb-2">강점</h4>
                            <ul class="list-disc list-inside text-sm text-gray-300 bg-gray-900/50 p-3 rounded-md space-y-1">${data.strengths.map(item => `<li>${item}</li>`).join('')}</ul>
                        </div>
                        <div>
                            <h4 class="font-semibold text-red-400 mb-2">개선점</h4>
                             <ul class="list-disc list-inside text-sm text-gray-300 bg-gray-900/50 p-3 rounded-md space-y-1">${data.weaknesses.map(item => `<li>${item}</li>`).join('')}</ul>
                        </div>
                    </div>
                </div>
                <div class="flex justify-center gap-4 mt-8">
                    <button onclick="closeSummaryModal()" class="bg-gray-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-gray-700 transition-colors">닫기</button>
                    <button onclick="showReflectionInput()" class="bg-indigo-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-indigo-700 transition-colors">다음 단계: 최종 피드백 생성</button>
                </div>
            `;
            renderRadarChart(data.scores);
        }

        function displayPracticeFinalReport(analysisData, reportData) {
            const scores = Object.values(analysisData.scores);
            const totalScore = Math.round(scores.reduce((a, b) => a + b, 0) / scores.length);

            ui.summaryModalContainer.innerHTML = `
                <div class="flex items-center justify-between mb-6">
                    <h3 class="text-2xl font-bold text-white">AI 코치의 최종 리포트</h3>
                    <button onclick="closeAllModals()" class="text-gray-400 hover:text-white transition-colors">
                        <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
                <div class="max-h-[70vh] overflow-y-auto pr-4">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center mb-8">
                        <div><canvas id="coaching-chart"></canvas></div>
                        <div class="space-y-6">
                            <div class="text-center bg-gray-700/50 p-4 rounded-lg">
                                <p class="text-gray-400 text-sm">총점</p>
                                <p class="text-4xl font-bold text-indigo-400">${totalScore}<span class="text-2xl text-gray-400">점</span></p>
                            </div>
                            <div>
                                <h4 class="font-semibold text-green-400 mb-2">강점</h4>
                                <ul class="list-disc list-inside text-sm text-gray-300 bg-gray-900/50 p-3 rounded-md space-y-1">${analysisData.strengths.map(item => `<li>${item}</li>`).join('')}</ul>
                            </div>
                            <div>
                                <h4 class="font-semibold text-red-400 mb-2">개선점</h4>
                                <ul class="list-disc list-inside text-sm text-gray-300 bg-gray-900/50 p-3 rounded-md space-y-1">${analysisData.weaknesses.map(item => `<li>${item}</li>`).join('')}</ul>
                            </div>
                        </div>
                    </div>
                    <hr class="border-gray-700 my-6">
                    <div class="space-y-6">
                        <div>
                            <h4 class="font-bold text-lg text-indigo-300 mb-2">종합 피드백</h4>
                            <div class="text-left text-gray-300 bg-gray-700/50 p-4 rounded-lg whitespace-pre-wrap">${reportData.finalFeedback}</div>
                        </div>
                        <div>
                            <h4 class="font-bold text-lg text-indigo-300 mb-2">개선된 코칭 스크립트 예시</h4>
                            <div class="text-left text-gray-300 bg-gray-700/50 p-4 rounded-lg whitespace-pre-wrap">${reportData.improvedScript}</div>
                        </div>
                    </div>
                </div>
                 <div class="flex justify-center gap-4 mt-8">
                    <button onclick="closeAllModals()" class="bg-gray-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-gray-700 transition-colors">닫기</button>
                    <button onclick="downloadReport()" class="bg-green-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-green-700 transition-colors">리포트 다운로드</button>
                </div>
            `;
            renderRadarChart(analysisData.scores);
        }

        function showReflectionInput() {
            ui.summaryModalContainer.innerHTML = `
                <h3 class="text-xl font-bold text-white mb-2">2단계: 대화 성찰하기 (선택)</h3>
                <p class="text-gray-400 mb-6">대화를 하면서 어떤 점이 가장 어려웠나요? 자유롭게 작성해주세요.<br>성찰 내용을 바탕으로 AI가 더 정확한 최종 피드백을 제공합니다.</p>
                <textarea id="reflection-input" rows="6" class="w-full bg-gray-700 text-white border border-gray-600 rounded-lg py-2 px-3 focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="예) 팀원의 진짜 속마음을 파악하기가 어려웠습니다. 어떤 질문을 해야 할지 막막하게 느껴졌어요."></textarea>
                <div class="flex justify-center gap-4 mt-6">
                    <button onclick="submitReflection(true)" class="bg-gray-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-gray-700 transition-colors">건너뛰기</button>
                    <button onclick="submitReflection(false)" class="bg-indigo-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-indigo-700 transition-colors">피드백 제출하기</button>
                </div>
            `;
        }

        function displayFinalReport(reportText) {
            ui.summaryModalContainer.innerHTML = `
                <div class="flex items-center justify-between mb-4">
                    <h3 class="text-xl font-bold text-white">3단계: AI 코치의 최종 피드백</h3>
                    <button onclick="closeAllModals()" class="text-gray-400 hover:text-white transition-colors">
                        <svg class="w-7 h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
                <div class="text-left text-gray-300 bg-gray-700/50 p-5 rounded-lg max-h-96 overflow-y-auto whitespace-pre-wrap">${reportText}</div>
                <div class="flex justify-end mt-6">
                    <button onclick="closeAllModals()" class="bg-indigo-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-indigo-700 transition-colors">닫기</button>
                </div>
            `;
        }

        // -----------------------------------------------------------------------------------
        // 7. UTILITY & HELPER FUNCTIONS (유틸리티 및 헬퍼 함수)
        // -----------------------------------------------------------------------------------
        
        function resetChatState() {
            conversationHistory = [];
            finalReportData = null;
            chartImageData = null;
            if (isRecording) {
                recognition.stop();
            }
        }
        
        function getContextPrompt() {
            return currentMode === 'simulation' 
                ? `**코칭 대상 팀원 정보:**\n- 유형: ${currentCoachingType} (${personas[currentCoachingType].name})\n- 특징: ${personas[currentCoachingType].characteristics}`
                : `**코칭 상황:**\n- 팀원: ${practiceContext.teamMember}\n- 주제: ${practiceContext.topic}\n- 생성된 페르소나: ${practiceContext.personaDescription}`;
        }
        
        function getAttitudeModifier() {
            const positiveKeywords = ['감사합니다', '좋네요', '도움이 됐어요', '알겠습니다', '해볼게요'];
            const positiveInteractionCount = conversationHistory.filter(m => 
                m.role === 'user' && positiveKeywords.some(kw => m.parts[0].text.includes(kw))
            ).length;

            if (positiveInteractionCount > 4) return '매우 긍정적이고 협조적인 태도로 응답해주세요.';
            if (positiveInteractionCount > 2) return '조금 더 긍정적이고 개방적인 태도로 응답해주세요.';
            return '';
        }

        function getTraitsDescription(descriptive = false) {
            const traitMap = {
                ability: { name: "역량(Ability)", values: { '-2': '매우 낮음', '-1': '낮음', '0': '보통', '1': '높음', '2': '매우 높음' } },
                motivation: { name: "동기(Motivation)", values: { '-2': '매우 낮음', '-1': '낮음', '0': '보통', '1': '높음', '2': '매우 높음' } },
                opportunity: { name: "기회(Opportunity)", values: { '-2': '매우 부족', '-1': '부족', '0': '보통', '1': '충분', '2': '매우 충분' } },
                energy: { name: "에너지", values: { '-2': '매우 외향적', '-1': '외향적', '0': '중립적', '1': '내향적', '2': '매우 내향적' } },
                logic: { name: "판단", values: { '-2': '매우 논리적', '-1': '논리적', '0': '균형적', '1': '감성적', '2': '매우 감성적' } },
                talk: { name: "대화", values: { '-2': '매우 단답형', '-1': '단답형', '0': '상황에 따라 다름', '1': '설명형', '2': '매우 설명형' } },
                assertive: { name: "주장", values: { '-2': '매우 자기주장적', '-1': '자기주장적', '0': '상황에 따라 다름', '1': '대세에 따르는 편', '2': '매우 대세에 따름' } },
                receptive: { name: "수용", values: { '-2': '매우 수용적', '-1': '수용적', '0': '일반적', '1': '비판적', '2': '매우 비판적/배척적' } }
            };

            if (descriptive) {
                return Object.keys(practiceContext.traits).map(key => 
                    `- ${traitMap[key].name}: ${traitMap[key].values[practiceContext.traits[key]]}`
                ).join('\n');
            }
            return Object.keys(traitMap).map(key => `${traitMap[key].name}: ${traitMap[key].values[practiceContext.traits[key]]}`).join(', ');
        }
        
        function renderRadarChart(scoresData) {
            const ctx = document.getElementById('coaching-chart')?.getContext('2d');
            if (!ctx) return;
            if (coachingChart) coachingChart.destroy();
            
            coachingChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['경청', '질문', '공감', '인정/지지', '행동촉진'],
                    datasets: [{
                        label: '나의 코칭 역량',
                        data: Object.values(scoresData),
                        backgroundColor: 'rgba(79, 70, 229, 0.2)',
                        borderColor: 'rgba(99, 102, 241, 1)',
                        pointBackgroundColor: 'rgba(99, 102, 241, 1)'
                    }]
                },
                options: {
                    scales: { r: { angleLines: { color: 'rgba(255, 255, 255, 0.2)' }, grid: { color: 'rgba(255, 255, 255, 0.2)' }, pointLabels: { color: '#fff', font: { size: 14 } }, ticks: { color: 'rgba(255, 255, 255, 0.7)', backdropColor: 'rgba(0, 0, 0, 0)', stepSize: 20 }, min: 0, max: 100 } },
                    plugins: { legend: { display: false } },
                    animation: { onComplete: () => { if (coachingChart) chartImageData = coachingChart.toBase64Image(); } }
                }
            });
        }
        
        // --- MODAL & UI HELPERS ---
        function showChatModal() {
            ui.chatModalBackdrop.classList.remove('hidden');
            setTimeout(() => {
                ui.chatModalBackdrop.classList.remove('opacity-0');
                ui.chatModalContainer.classList.remove('opacity-0', 'scale-95');
            }, 10);
        }

        function closeChatModal() {
            ui.chatModalBackdrop.classList.add('opacity-0');
            ui.chatModalContainer.classList.add('opacity-0', 'scale-95');
            setTimeout(() => ui.chatModalBackdrop.classList.add('hidden'), 300);
        }

        function showSummaryModal() {
            ui.summaryModalContainer.innerHTML = '';
            ui.summaryModalBackdrop.classList.remove('hidden');
            setTimeout(() => {
                ui.summaryModalBackdrop.classList.remove('opacity-0');
                ui.summaryModalContainer.classList.remove('opacity-0', 'scale-95');
            }, 10);
        }
        
        function closeSummaryModal() {
             ui.summaryModalBackdrop.classList.add('opacity-0');
             ui.summaryModalContainer.classList.add('opacity-0', 'scale-95');
             setTimeout(() => ui.summaryModalBackdrop.classList.add('hidden'), 300);
        }

        function closeAllModals() {
             closeSummaryModal();
             closeChatModal();
        }

        function setSendButtonLoading(isLoading) {
            ui.sendButton.disabled = isLoading;
            if (isLoading) {
                ui.sendButton.innerHTML = `<svg class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg>`;
            } else {
                ui.sendButton.innerHTML = '전송';
            }
        }
        
        function setSummaryModalLoading(message) {
            ui.summaryModalContainer.innerHTML = `<div class="text-center text-gray-400 animate-pulse py-10"><p>${message}</p></div>`;
        }
        
        function displayAnalysisUnavailable() {
            ui.summaryModalContainer.innerHTML = `
                <h3 class="text-xl font-bold text-white text-center mb-4">분석 불가</h3>
                <p class="text-gray-400 text-center mb-8">코칭 지수를 분석하기에는 대화 내용이 충분하지 않습니다.<br>리더님의 답변이 3회 이상일 때 분석이 가능합니다.</p>
                <div class="flex justify-center gap-4">
                    <button onclick="closeSummaryModal()" class="bg-gray-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-gray-700 transition-colors">대화로 돌아가기</button>
                    <button onclick="closeAllModals()" class="bg-indigo-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-indigo-700 transition-colors">대화 끝내기</button>
                </div>
            `;
        }

        function displayAnalysisError() {
            ui.summaryModalContainer.innerHTML = `
                <h3 class="text-xl font-bold text-white text-center mb-4">분석 오류</h3>
                <p class="text-gray-400 text-center mb-8">AI 리포트 생성 중 오류가 발생했습니다.<br>잠시 후 다시 시도해주세요.</p>
                <div class="flex justify-center">
                    <button onclick="closeSummaryModal()" class="bg-indigo-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-indigo-700 transition-colors">닫기</button>
                </div>
            `;
        }
        
        function updateChatInfoTooltip() {
            const tooltip = document.getElementById('chat-info-tooltip');
            const content = currentMode === 'simulation'
                ? `<p class="font-bold mb-2">${personas[currentCoachingType].name} (${currentCoachingType})</p><p class="text-xs"><strong class="font-semibold">목표:</strong> ${personas[currentCoachingType].coachingGoal}</p>`
                : `<p class="font-bold mb-2">${practiceContext.teamMember}</p><p class="text-xs"><strong class="font-semibold">목표:</strong> ${practiceContext.goal}</p>`;
            tooltip.innerHTML = content;
        }

        function toggleFeedback(isChecked) {
            isFeedbackEnabled = isChecked;
            ui.feedbackContent.innerHTML = `<div class="text-center text-gray-500 pt-10"><p>${isChecked ? '실시간 피드백이 활성화되었습니다.' : '실시간 피드백이 비활성화되었습니다.<br>대화에 더 집중해보세요.'}</p></div>`;
        }

        function displayPracticeSummary() {
            document.getElementById('practice-summary-text').innerHTML = `
                <p><strong class="font-semibold text-gray-300 w-24 inline-block">팀원:</strong> ${practiceContext.teamMember}</p>
                <p><strong class="font-semibold text-gray-300 w-24 inline-block">코칭 주제:</strong> ${practiceContext.topic}</p>
                <p><strong class="font-semibold text-gray-300 w-24 inline-block">코칭 목표:</strong> ${practiceContext.goal}</p>
            `;
        }

        function returnToPracticeSetup() {
            ui.practiceSummaryView.classList.add('hidden');
            ui.practiceSetupView.classList.remove('hidden');
        }
        
        function applySuggestion(button) {
            ui.chatInput.value = button.textContent;
            ui.chatInput.focus();
        }

        // -----------------------------------------------------------------------------------
        // 8. EXTERNAL LIBRARIES & FEATURES (외부 라이브러리 및 기능)
        // -----------------------------------------------------------------------------------

        function downloadReport() {
            if (currentMode !== 'practice' || !finalReportData || !chartImageData) {
                alert("리포트를 다운로드할 수 없습니다. 리포트 데이터가 완전하지 않습니다.");
                console.error("Download Error:", { mode: currentMode, reportData: !!finalReportData, chartData: !!chartImageData });
                return;
            }

            const { Packer, Document, Paragraph, HeadingLevel, TextRun, ImageRun, AlignmentType } = docx;
            const { finalFeedback, improvedScript } = finalReportData;
            
            fetch(chartImageData)
                .then(res => res.blob())
                .then(blob => {
                     const doc = new Document({
                        sections: [{
                            children: [
                                new Paragraph({ text: "AI 코칭 시뮬레이션 최종 리포트", heading: HeadingLevel.TITLE, alignment: AlignmentType.CENTER }),
                                new Paragraph({ text: `코칭 대상: ${practiceContext.teamMember}`, alignment: AlignmentType.CENTER, spacing: { after: 100 } }),
                                new Paragraph({ text: `코칭 주제: ${practiceContext.topic}`, alignment: AlignmentType.CENTER, spacing: { after: 400 } }),
                                new Paragraph({ text: "나의 코칭 지수", heading: HeadingLevel.HEADING_1, spacing: { before: 200, after: 200 } }),
                                new Paragraph({ children: [new ImageRun({ data: blob, transformation: { width: 450, height: 450 } })], alignment: AlignmentType.CENTER }),
                                new Paragraph({ text: "", pageBreakBefore: true }),
                                new Paragraph({ text: "종합 피드백", heading: HeadingLevel.HEADING_1, spacing: { before: 200, after: 200 } }),
                                ...finalFeedback.split('\n').map(line => new Paragraph({ text: line, spacing: { after: 100 } })),
                                new Paragraph({ text: "", spacing: { after: 400 } }),
                                new Paragraph({ text: "개선된 코칭 스크립트 예시", heading: HeadingLevel.HEADING_1, spacing: { before: 200, after: 200 } }),
                                ...improvedScript.split('\n').map(line => {
                                    const parts = line.split(':');
                                    if (parts.length > 1 && (parts[0].includes('리더') || parts[0].includes('팀원'))) {
                                        return new Paragraph({
                                            children: [ new TextRun({ text: `${parts[0]}:`, bold: true }), new TextRun({ text: parts.slice(1).join(':') }) ],
                                            spacing: { after: 100 }
                                        });
                                    }
                                    return new Paragraph({ text: line, spacing: { after: 100 } });
                                }),
                            ],
                        }],
                    });

                    Packer.toBlob(doc).then(docBlob => {
                        saveAs(docBlob, '엑코치_코칭리포트.docx');
                    });
                }).catch(err => {
                    console.error("Error creating blob from chart image:", err);
                    alert("차트 이미지 변환 중 오류가 발생하여 리포트를 생성할 수 없습니다.");
                });
        }
        
        function setupSpeechRecognition() {
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            if (!SpeechRecognition) {
                console.log('Speech Recognition not supported');
                return;
            }
            
            recognition = new SpeechRecognition();
            recognition.continuous = false;
            recognition.lang = 'ko-KR';
            recognition.interimResults = false;
            recognition.maxAlternatives = 1;

            recognition.onstart = () => {
                isRecording = true;
                document.getElementById('mic-icon').classList.add('text-red-500');
            };

            recognition.onend = () => {
                isRecording = false;
                document.getElementById('mic-icon').classList.remove('text-red-500');
            };

            recognition.onresult = (event) => {
                ui.chatInput.value = event.results[0][0].transcript;
                handleSendMessage();
            };
            
            recognition.onerror = (event) => {
                console.error('Speech recognition error:', event.error);
                isRecording = false;
                document.getElementById('mic-icon').classList.remove('text-red-500');
            };
        }

        function toggleVoiceInput() {
            if (!recognition) {
                alert('음성 인식이 지원되지 않는 브라우저입니다.');
                return;
            }
            isRecording ? recognition.stop() : recognition.start();
        }
        
        function handleAudioUpload(files) {
            if (files.length === 0) return;
            document.getElementById('audio-file-name').textContent = `선택된 파일: ${files[0].name}`;
            
            showSummaryModal();
            setSummaryModalLoading("AI 코치가 음성 파일을 분석하여<br>코칭 지수를 생성하고 있습니다...");
            
            // NOTE: This is a placeholder for actual audio analysis.
            // In a real implementation, you would upload the file and process it.
            setTimeout(async () => { 
                try {
                    const fakeAnalysisData = await getCoachingAnalysis(); // Using chat analysis as a stand-in
                    displayCoachingIndex(fakeAnalysisData);
                } catch (error) {
                    displayAnalysisError();
                }
            }, 3000);
        }

        // -----------------------------------------------------------------------------------
        // 9. INITIALIZATION (초기화)
        // -----------------------------------------------------------------------------------

        function initializeUI() {
            ui.landingPage = document.getElementById('landing-page');
            ui.mainApp = document.getElementById('main-app');
            ui.chatModalBackdrop = document.getElementById('chat-modal-backdrop');
            ui.chatModalContainer = document.getElementById('chat-modal-container');
            ui.chatModalTitle = document.getElementById('chat-modal-title');
            ui.chatModalSubtitle = document.getElementById('chat-modal-subtitle');
            ui.chatRecommendations = document.getElementById('chat-recommendations');
            ui.chatAndFeedbackInterface = document.getElementById('chat-and-feedback-interface');
            ui.personaNameRole = document.getElementById('persona-name-role');
            ui.personaCharacteristics = document.getElementById('persona-characteristics');
            ui.personaSituation = document.getElementById('persona-situation');
            ui.recommendationTags = document.getElementById('recommendation-tags');
            ui.coachingGoalText = document.getElementById('coaching-goal-text');
            ui.chatMessages = document.getElementById('chat-messages');
            ui.chatInput = document.getElementById('chat-input');
            ui.sendButton = document.getElementById('send-button');
            ui.feedbackContent = document.getElementById('feedback-content');
            ui.summaryModalBackdrop = document.getElementById('summary-modal-backdrop');
            ui.summaryModalContainer = document.getElementById('summary-modal-container');
            ui.teamMemberNamePosInput = document.getElementById('team-member-name-pos');
            ui.conversationTopicInput = document.getElementById('conversation-topic');
            ui.coachingGoalPracticeInput = document.getElementById('coaching-goal-practice');
            ui.otherInfoPracticeInput = document.getElementById('other-info-practice');
            ui.practiceSetupView = document.getElementById('practice-setup-view');
            ui.practiceSummaryView = document.getElementById('practice-summary-view');
        }
        
        function addEventListeners() {
            ui.sendButton.addEventListener('click', handleSendMessage);
            ui.chatInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    handleSendMessage();
                }
            });
        }
        
        function enterApp() {
            ui.landingPage.classList.add('hidden');
            ui.mainApp.classList.remove('hidden');
        }

        document.addEventListener('DOMContentLoaded', () => {
            initializeUI();
            addEventListeners();
            setupSpeechRecognition();
            showTab('simulation');
        });

    </script>
</body>
</html>

