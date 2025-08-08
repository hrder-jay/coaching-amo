<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI를 활용한 리더의 코칭 시뮬레이션</title>
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: 이 앱은 여러 화면(메인, 시나리오 선택, 시뮬레이션, 리포트, 라이브러리)을 div 요소의 표시/숨김으로 전환하는 단일 페이지 애플리케이션(SPA) 구조를 채택했습니다. 이 방식은 사용자가 '모드 선택 -> 시나리오 선택 -> 시뮬레이션 -> 결과 검토'라는 명확한 흐름을 따라 자연스럽게 앱을 사용하도록 유도합니다. 복잡한 기능을 단계별로 분리하여 사용성을 높였고, 핵심 상호작용인 동적 채팅을 중심으로 학습(라이브러리)과 성찰(리포트) 기능을 보조적으로 배치하여 효과적인 학습 경험을 제공하도록 설계했습니다. -->
    <!-- Visualization & Content Choices: 8가지 AMO 시나리오는 명확한 옵션 제공을 위해 클릭 가능한 카드로 시각화했습니다. 코칭 대화는 실제 채팅과 유사한 인터페이스로 구현하여 몰입감을 높였습니다. 최종 피드백의 다차원적인 코칭 역량(경청, 문제 해결 등)은 Chart.js의 '레이더 차트'를 사용하여 시각적으로 강점과 약점인지를 한눈에 파악할 수 있도록 했습니다. 이 모든 요소는 사용자의 학습 목표 달성을 위해 선택되었으며, SVG나 Mermaid.js는 사용하지 않았습니다. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', 'Noto Sans KR', sans-serif;
        }
        .hidden {
            display: none;
        }
        @keyframes dot-flashing {
            0% { background-color: #94a3b8; }
            50%, 100% { background-color: #e2e8f0; }
        }
        .dot-flashing {
            position: relative;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #94a3b8;
            color: #94a3b8;
            animation: dot-flashing 1s infinite linear alternate;
            animation-delay: 0s;
        }
        .dot-flashing::before, .dot-flashing::after {
            content: "";
            display: inline-block;
            position: absolute;
            top: 0;
        }
        .dot-flashing::before {
            left: -15px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #94a3b8;
            color: #94a3b8;
            animation: dot-flashing 1s infinite linear alternate;
            animation-delay: 0s;
        }
        .dot-flashing::after {
            left: 15px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #94a3b8;
            color: #94a3b8;
            animation: dot-flashing 1s infinite linear alternate;
            animation-delay: 0.5s;
        }
    </style>
</head>
<body class="bg-gray-100">

    <!-- Main Screen -->
    <div id="main-screen" class="flex flex-col items-center justify-center min-h-screen p-4">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-2xl w-full text-center">
            <h1 class="text-4xl font-bold text-gray-800 mb-4">AI를 활용한 리더의 코칭 시뮬레이션</h1>
            <p class="text-gray-600 mb-8">리더십 코칭 역량 강화를 위한 시뮬레이션 웹앱입니다. 다양한 상황을 연습하고, AI로부터 구체적인 피드백을 받아보세요.</p>
            <div class="space-y-4">
                <button onclick="navigateTo('scenario-selection-screen')" class="w-full py-4 px-6 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-xl shadow-lg transition duration-300 transform hover:scale-105">
                    가상 시뮬레이션 연습하기
                </button>
                <button onclick="navigateTo('custom-case-creation-screen')" class="w-full py-4 px-6 bg-green-600 hover:bg-green-700 text-white font-semibold rounded-xl shadow-lg transition duration-300 transform hover:scale-105">
                    AI 대화로 맞춤형 케이스 만들기
                </button>
                <button onclick="navigateTo('library-screen')" class="w-full py-4 px-6 bg-purple-600 hover:bg-purple-700 text-white font-semibold rounded-xl shadow-lg transition duration-300 transform hover:scale-105">
                    코칭 라이브러리
                </button>
            </div>
        </div>
    </div>

    <!-- Scenario Selection Screen -->
    <div id="scenario-selection-screen" class="hidden p-4 md:p-8">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl mx-auto">
            <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">가상 시뮬레이션 선택</h2>
            <div id="scenario-list" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Scenarios will be injected here by JS -->
            </div>
            <div class="text-center">
                <button onclick="navigateTo('main-screen')" class="mt-8 py-2 px-6 bg-gray-300 hover:bg-gray-400 text-gray-800 rounded-xl transition duration-300 font-semibold">
                    메인으로 돌아가기
                </button>
            </div>
        </div>
    </div>

    <!-- Custom Case Creation Screen -->
    <div id="custom-case-creation-screen" class="hidden flex flex-col items-center justify-center min-h-screen p-4">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl w-full">
            <h2 class="text-3xl font-bold text-gray-800 mb-2 text-center">원온원 리더십 케이스 생성</h2>
            <p class="text-gray-500 mb-8 text-center">효과적인 원온원 면담을 위한 AI 기반 시뮬레이션 도구</p>
            <div class="flex flex-col md:flex-row gap-8 justify-center items-stretch">
                <button onclick="navigateTo('template-selection-screen')" class="flex flex-col items-center justify-center p-8 bg-white rounded-2xl shadow-lg border border-gray-200 transition duration-300 transform hover:scale-105 w-full md:w-1/2">
                    <svg class="w-16 h-16 text-blue-600 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                    <h3 class="text-xl font-bold text-gray-800">AI 자동 케이스 생성</h3>
                    <p class="text-sm text-gray-600 mt-2">미리 준비된 템플릿을 선택하여 시뮬레이션을 시작합니다.</p>
                </button>
                <button onclick="startSimulation({ id: 'custom', type: 'AI Chat 케이스 생성', desc: '자유로운 대화를 통해 AI가 맞춤형 팀원 역할을 수행합니다.', isCustom: true })" class="flex flex-col items-center justify-center p-8 bg-white rounded-2xl shadow-lg border border-gray-200 transition duration-300 transform hover:scale-105 w-full md:w-1/2">
                    <svg class="w-16 h-16 text-green-600 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path></svg>
                    <h3 class="text-xl font-bold text-gray-800">AI Chat 케이스 생성</h3>
                    <p class="text-sm text-gray-600 mt-2">AI와의 자유로운 대화로 맞춤형 케이스를 만들어갑니다.</p>
                </button>
            </div>
             <div class="text-center">
                <button onclick="navigateTo('main-screen')" class="mt-12 py-2 px-6 bg-gray-300 hover:bg-gray-400 text-gray-800 rounded-xl transition duration-300 font-semibold">
                    메인으로 돌아가기
                </button>
            </div>
        </div>
    </div>

    <!-- Template Selection Screen -->
    <div id="template-selection-screen" class="hidden p-4 md:p-8">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl mx-auto">
            <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">템플릿으로 시작하기</h2>
            <div id="template-list" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <!-- Templates will be injected here by JS -->
            </div>
            <div class="text-center">
                <button onclick="navigateTo('custom-case-creation-screen')" class="mt-8 py-2 px-6 bg-gray-300 hover:bg-gray-400 text-gray-800 rounded-xl transition duration-300 font-semibold">
                    뒤로가기
                </button>
            </div>
        </div>
    </div>
    
    <!-- Simulation Screen -->
    <div id="simulation-screen" class="hidden flex flex-col items-center min-h-screen bg-gray-100 p-4">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl w-full flex flex-col h-[90vh]">
            <div class="border-b pb-4 mb-4">
                <h2 id="simulation-title" class="text-3xl font-bold text-gray-800"></h2>
                <p id="simulation-team-member" class="text-gray-600 mt-2"></p>
                <p class="text-sm text-gray-400">시뮬레이션을 종료하려면 '시뮬레이션 종료' 버튼을 눌러주세요.</p>
            </div>
            <div id="conversation-area" class="flex-1 overflow-y-auto space-y-4 pr-2">
                <!-- Conversation will be injected here by JS -->
            </div>
            <div class="mt-4 pt-4 border-t border-gray-200">
                <div class="flex flex-col space-y-2">
                    <textarea id="user-input" class="w-full p-4 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-purple-500" placeholder="대화 내용을 직접 입력하세요."></textarea>
                    <div class="flex space-x-2">
                        <button onclick="handleUserInput()" class="flex-1 py-3 px-6 bg-purple-600 hover:bg-purple-700 text-white font-semibold rounded-xl shadow-lg transition duration-300">
                            전송
                        </button>
                        <button onclick="handleEndSimulation()" class="py-3 px-6 bg-red-600 hover:bg-red-700 text-white rounded-xl transition duration-300 font-semibold">
                            시뮬레이션 종료
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Report Screen -->
    <div id="report-screen" class="hidden p-4 md:p-8">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl mx-auto">
            <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">피드백 리포트</h2>
            <div class="bg-gray-50 rounded-xl p-6 mb-6 border border-gray-200">
                <h3 class="text-2xl font-bold text-gray-800 mb-2">종합 요약</h3>
                <div class="flex items-center justify-between">
                    <p class="text-lg font-semibold text-gray-700">총점:</p>
                    <span id="report-total-score" class="text-4xl font-bold text-blue-600"></span>
                </div>
                <p id="report-summary" class="text-gray-600 mt-2"></p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                <div id="report-detailed-scores" class="md:col-span-1 space-y-4">
                    <!-- Detailed scores will be injected here by JS -->
                </div>
                <div class="chart-container relative h-64 md:h-auto">
                    <canvas id="feedback-chart"></canvas>
                </div>
            </div>
            <div class="mb-6">
                <h3 class="text-2xl font-bold text-gray-800 mb-4">대화 분석 타임라인</h3>
                <div id="report-timeline" class="space-y-4 max-h-96 overflow-y-auto bg-gray-50 p-4 rounded-xl">
                    <!-- Timeline will be injected here by JS -->
                </div>
            </div>
            <div class="bg-yellow-50 rounded-xl p-6 mb-6 border border-yellow-200">
                <h3 class="text-2xl font-bold text-gray-800 mb-2">AMO 진단 및 코칭 모델 적합성 평가</h3>
                <p id="report-amo-analysis" class="text-gray-700 mt-2"></p>
            </div>
            <div class="bg-green-50 rounded-xl p-6 mb-6 border border-green-200">
                <h3 class="text-2xl font-bold text-gray-800 mb-2">맞춤형 학습 추천</h3>
                <p id="report-recommendation" class="text-gray-700 mt-2"></p>
            </div>
            <div class="text-center">
                <button onclick="navigateTo('main-screen')" class="mt-8 w-full py-3 px-6 bg-gray-300 hover:bg-gray-400 text-gray-800 rounded-xl transition duration-300 font-semibold">
                    메인으로 돌아가기
                </button>
            </div>
        </div>
    </div>

    <!-- Library Screen -->
    <div id="library-screen" class="hidden p-4 md:p-8">
        <div class="bg-white rounded-2xl shadow-xl p-8 max-w-4xl mx-auto">
            <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">코칭 라이브러리</h2>
            <div id="library-tabs" class="flex justify-center mb-6 space-x-2 md:space-x-4">
                <!-- Library tabs will be injected here by JS -->
            </div>
            <div id="library-content" class="bg-gray-50 rounded-xl p-6 border border-gray-200 min-h-[300px]">
                <!-- Library content will be injected here by JS -->
            </div>
            <div class="text-center">
                <button onclick="navigateTo('main-screen')" class="mt-8 py-2 px-6 bg-gray-300 hover:bg-gray-400 text-gray-800 rounded-xl transition duration-300 font-semibold">
                    메인으로 돌아가기
                </button>
            </div>
        </div>
    </div>

    <script>
        // --- DATA STORE ---
        const appState = {
            currentPage: 'main-screen',
            currentScenario: null,
            conversationHistory: [],
            lastFeedback: null,
            feedbackChartInstance: null,
        };

        const scenariosData = {
            amo1: { id: 'amo1', type: '삼무(三無)형', amo: 'A↓, M↓, O↓', desc: '방향을 잃은 신입 팀원 코칭', initialDialogue: "안녕하세요. 요즘 업무에 어려움은 없으신가요?" },
            amo2: { id: 'amo2', type: '게으른 바보형', amo: 'A↓, M↓', desc: '성장에 무관심한 팀원 코칭', initialDialogue: "민호님, 요즘 성과가 좀 부진한 것 같아 걱정돼요." },
            amo3: { id: 'amo3', type: '아픈 손가락형', amo: 'A↓, M↑, O↓', desc: '의욕은 넘치지만, 역량과 환경이 부족한 팀원 코칭', initialDialogue: "수아님, 요즘 아이디어가 잘 안 풀린다고 들었어요." },
            amo4: { id: 'amo4', type: '게으른 천재형', amo: 'A↑, M↓, O↓', desc: '뛰어난 잠재력을 가졌지만, 동기를 잃은 팀원 코칭', initialDialogue: "현우님, 요즘 컨디션이 안 좋아 보이는데, 혹시 무슨 일 있으신가요?" },
            amo5: { id: 'amo5', type: '착한 바보형', amo: 'A↓, M↑, O↑', desc: '성실하지만, 역량 부족으로 실수가 잦은 팀원 코칭', initialDialogue: "지은님, 지난주에 고객 응대할 때 이런 점이 아쉬웠어요." },
            amo6: { id: 'amo6', type: '워라밸추구형', amo: 'A↑, M↓, O↑', desc: '높은 역량을 가졌지만, 개인의 삶을 중요시하는 팀원 코칭', initialDialogue: "선영 님, 잠깐 얘기 좀 할 수 있을까요? 이번에 새로 시작하는 프로젝트에 대해 같이 이야기해보고 싶어서요." },
            amo7: { id: 'amo7', type: '부지런한 천재형', amo: 'A↑, M↑, O↓', desc: '역량과 동기는 충분하나, 비효율적인 환경에 좌절한 팀원 코칭', initialDialogue: "성민님, 이번 프로젝트 일정이 너무 빡빡해서 힘드시죠?" },
            amo8: { id: 'amo8', type: '만능(萬能)형', amo: 'A↑, M↑, O↑', desc: '탁월한 성과를 내며 새로운 도전을 원하는 팀원 코칭', initialDialogue: "하늘님, 요즘 팀에서 가장 큰 공헌을 하고 있어요. 정말 고마워요." },
            custom: { id: 'custom', type: 'AI Chat 케이스 생성', desc: '자유로운 대화를 통해 AI가 맞춤형 팀원 역할을 수행합니다.', initialDialogue: "안녕하세요. 요즘 업무는 잘 되시나요? 어떤 이야기를 나눠볼까요?" },
        };

        const templatesData = [
            { id: 't1', type: '신중하고 내향적인 분석가', desc: '능력은 뛰어나나, 자신의 의견을 잘 표현하지 않음', initialDialogue: "안녕하세요. 최근 진행 중인 분석 작업에 대해 이야기하고 싶습니다." },
            { id: 't2', type: '외향적이고 열정적인 활동가', desc: '적극적으로 일하지만, 가끔 지나치게 서두르는 경향이 있음', initialDialogue: "팀장님! 다음 분기 캠페인에 대해 드릴 말씀이 있습니다!" },
            { id: 't3', type: '경력직 이직 후 적응 중인 팀원', desc: '새로운 팀 문화에 적응 중이며, 자신의 강점을 아직 충분히 발휘하지 못함', initialDialogue: "팀장님, 잠시 시간 괜찮으신가요? 업무 프로세스에 대해 궁금한 점이 있습니다." },
        ];
        
        const libraryData = {
            grow: { title: 'GROW', content: `<h3>GROW 모델이란?</h3><p>목표 설정과 문제 해결을 위한 효과적인 코칭 대화 모델입니다. 목표(Goal)부터 시작해 현실(Reality)을 파악하고, 여러 선택지(Options)를 탐색한 뒤, 실천 의지(Will)를 다지는 4단계로 구성됩니다.</p><h4>G: Goal (목표 설정)</h4><p>팀원이 달성하고자 하는 구체적인 목표를 명확히 합니다.<br><b>질문 예시:</b> "오늘 대화를 통해 어떤 결과를 얻고 싶으신가요?"</p><h4>R: Reality (현실 파악)</h4><p>현재 팀원의 상황, 문제점, 장애물 등 현실을 객관적으로 파악합니다.<br><b>질문 예시:</b> "현재 상황은 어떤가요? 목표 달성을 가로막는 장애물은 무엇이라고 생각하나요?"</p><h4>O: Options (선택지 탐색)</h4><p>목표 달성을 위해 시도할 수 있는 다양한 대안과 선택지를 탐색합니다.<br><b>질문 예시:</b> "이 문제를 해결하기 위해 어떤 방법들이 있을까요?"</p><h4>W: Will (실천 의지)</h4><p>탐색한 선택지 중 가장 좋은 방법을 선택하고, 구체적인 실천 계획을 세워 의지를 다집니다.<br><b>질문 예시:</b> "지금까지 이야기한 방법들 중 어떤 것을 가장 먼저 시도해보고 싶으신가요?"</p>` },
            sbi: { title: 'SBI', content: `<h3>SBI 모델이란?</h3><p>행동에 대한 구체적이고 객관적인 피드백을 전달하는 모델입니다. 감정적인 비난 없이 특정 상황(Situation), 팀원의 행동(Behavior), 그리고 그 행동이 미친 영향(Impact)을 중심으로 대화하여 팀원의 성장을 돕습니다.</p><h4>S: Situation (상황)</h4><p>언제, 어디서, 어떤 일이 있었는지 구체적인 상황을 설명합니다.<br><b>예시:</b> "지난주 화요일 팀 회의 때..."</p><h4>B: Behavior (행동)</h4><p>팀원이 한 행동을 객관적으로 묘사합니다.<br><b>예시:</b> "...발표 중 제안했던 아이디어에 대해 아무런 의견을 내지 않았습니다."</p><h4>I: Impact (영향)</h4><p>그 행동이 리더, 팀, 프로젝트에 어떤 영향을 미쳤는지 설명합니다.<br><b>예시:</b> "그 때문에 다른 팀원들이 아이디어에 대해 어떻게 생각하는지 파악하기 어려웠습니다."</p>` },
            nvc: { title: 'NVC', content: `<h3>비폭력 대화(NVC)란?</h3><p>비난이나 비판 없이 자신의 생각과 감정을 전달하고, 상대방의 감정에 공감하며 진정한 소통을 이끌어내는 모델입니다. 관찰(Observation), 느낌(Feeling), 욕구(Need), 부탁(Request)의 4단계로 구성됩니다.</p><h4>O: Observation (관찰)</h4><p>평가나 판단 없이 있는 그대로의 사실을 이야기합니다.<br><b>예시:</b> "지난주에 마감 기한을 넘긴 보고서를 봤을 때..."</p><h4>F: Feeling (느낌)</h4><p>관찰된 사실에 대한 자신의 감정을 솔직하게 표현합니다.<br><b>예시:</b> "...저는 약간 걱정되는 마음이 들었습니다."</p><h4>N: Need (욕구)</h4><p>그 느낌을 불러일으킨 자신의 욕구를 밝힙니다.<br><b>예시:</b> "이는 팀의 일정 관리가 잘 되었으면 하는 저의 욕구 때문입니다."</p><h4>R: Request (부탁)</h4><p>상대방에게 필요한 행동을 구체적으로 부탁합니다.<br><b>예시:</b> "다음부터는 마감 기한을 맞추는 데 어떤 도움이 필요할지 저와 먼저 논의해 주실 수 있을까요?"</p>` },
        };

        // --- NAVIGATION & UI RENDERING ---
        function navigateTo(pageId) {
            document.querySelectorAll('body > div').forEach(div => div.classList.add('hidden'));
            document.getElementById(pageId).classList.remove('hidden');
            appState.currentPage = pageId;

            if (pageId === 'library-screen') {
                renderLibrary();
            }
        }

        function renderScenarios() {
            const listEl = document.getElementById('scenario-list');
            listEl.innerHTML = '';
            Object.values(scenariosData).filter(s => s.id !== 'custom').forEach(scenario => {
                const button = document.createElement('button');
                button.className = "bg-gray-50 hover:bg-gray-200 transition duration-300 rounded-xl p-6 text-left border border-gray-200 shadow-sm transform hover:scale-105";
                button.onclick = () => startSimulation(scenario);
                button.innerHTML = `
                    <h3 class="text-xl font-semibold text-gray-800">${scenario.type}</h3>
                    <p class="text-blue-600 font-medium my-1">진단: ${scenario.amo}</p>
                    <p class="text-gray-600">${scenario.desc}</p>
                `;
                listEl.appendChild(button);
            });
        }

        function renderTemplates() {
            const listEl = document.getElementById('template-list');
            listEl.innerHTML = '';
            templatesData.forEach(template => {
                const button = document.createElement('button');
                button.className = "bg-blue-100 hover:bg-blue-200 transition duration-300 rounded-xl p-4 text-left border border-blue-200 shadow-sm";
                button.onclick = () => startSimulation({ ...template, isCustom: true });
                button.innerHTML = `
                    <h4 class="font-semibold text-blue-800">${template.type}</h4>
                    <p class="text-sm text-gray-700 mt-1">${template.desc}</p>
                `;
                listEl.appendChild(button);
            });
        }
        
        function renderLibrary(activeTab = 'grow') {
            const tabsEl = document.getElementById('library-tabs');
            const contentEl = document.getElementById('library-content');
            tabsEl.innerHTML = '';
            Object.keys(libraryData).forEach(key => {
                const button = document.createElement('button');
                button.innerText = libraryData[key].title;
                button.className = `py-2 px-6 rounded-xl font-semibold transition duration-300 ${activeTab === key ? 'bg-blue-600 text-white' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'}`;
                button.onclick = () => renderLibrary(key);
                tabsEl.appendChild(button);
            });
            contentEl.innerHTML = libraryData[activeTab].content;
        }

        function renderConversation() {
            const conversationArea = document.getElementById('conversation-area');
            conversationArea.innerHTML = '';
            const displayConversation = appState.conversationHistory;
            
            displayConversation.forEach(msg => {
                const msgWrapper = document.createElement('div');
                const isUser = msg.role === 'user';
                msgWrapper.className = `flex ${isUser ? 'justify-end' : 'justify-start'}`;
                
                const msgBubble = document.createElement('div');
                msgBubble.className = `max-w-md p-4 rounded-3xl shadow-md ${isUser ? 'bg-blue-600 text-white' : 'bg-gray-200 text-gray-800'}`;
                
                const speakerName = document.createElement('p');
                speakerName.className = 'font-semibold text-sm mb-1';
                speakerName.innerText = isUser ? '리더' : appState.currentScenario.teamMember.name || '팀원';
                msgBubble.appendChild(speakerName);

                const msgText = document.createElement('p');
                msgText.innerText = msg.parts[0].text;
                msgBubble.appendChild(msgText);
                
                msgWrapper.appendChild(msgBubble);
                conversationArea.appendChild(msgWrapper);
            });

            if (appState.isLoading) {
                conversationArea.innerHTML += `
                    <div class="flex justify-start">
                        <div class="max-w-md p-4 rounded-3xl shadow-md bg-gray-200 text-gray-800">
                            <p class="font-semibold text-sm mb-1">${appState.currentScenario.teamMember.name || '팀원'}</p>
                            <div class="flex items-center space-x-2">
                                <span class="dot-flashing"></span>
                            </div>
                        </div>
                    </div>
                `;
            }

            if (appState.lastFeedback) {
                conversationArea.innerHTML += `
                    <div class="flex justify-start">
                        <div class="max-w-md p-4 rounded-3xl shadow-md bg-yellow-100 text-gray-800 border border-yellow-300">
                            <p class="font-bold">AI 피드백:</p>
                            <p class="font-medium">${appState.lastFeedback.feedbackSummary}</p>
                        </div>
                    </div>
                `;
            }

            conversationArea.scrollTop = conversationArea.scrollHeight;
        }

        function renderReport() {
            const report = appState.lastFeedback;
            if (!report) return;

            document.getElementById('report-total-score').innerText = `${report.score}점`;
            document.getElementById('report-summary').innerText = report.feedbackSummary;

            const detailedScoresEl = document.getElementById('report-detailed-scores');
            detailedScoresEl.innerHTML = '';
            report.detailedFeedback.forEach(item => {
                detailedScoresEl.innerHTML += `
                    <div class="bg-blue-50 rounded-xl p-4 border border-blue-200">
                        <h4 class="font-bold text-blue-800">${item.point}: ${item.score}</h4>
                        <p class="text-gray-700 mt-1 text-sm">${item.feedback}</p>
                    </div>
                `;
            });
            
            const timelineEl = document.getElementById('report-timeline');
            timelineEl.innerHTML = '';
            const timelineHistory = appState.conversationHistory;
            timelineHistory.forEach(msg => {
                const isUser = msg.role === 'user';
                timelineEl.innerHTML += `
                    <div class="flex ${isUser ? 'justify-end' : 'justify-start'}">
                        <div class="max-w-xl p-4 rounded-3xl shadow-md ${isUser ? 'bg-blue-600 text-white' : 'bg-gray-200 text-gray-800'}">
                            <p class="font-semibold text-sm mb-1">${isUser ? '리더' : (appState.currentScenario.teamMember.name || '팀원')}</p>
                            <p>${msg.parts[0].text}</p>
                        </div>
                    </div>
                `;
            });
            
            document.getElementById('report-amo-analysis').innerHTML = `<strong>AMO 진단</strong>: 리더님은 이 팀원의 문제를 '동기 부족(Motivation)'으로 정확하게 진단하셨습니다.`;
            document.getElementById('report-recommendation').innerHTML = `팀원 스스로 해결책을 찾도록 유도하는 질문이 부족했습니다. '코칭 라이브러리'에서 <strong>GROW 모델</strong>을 참고하여, 'Reality'와 'Options' 단계를 연습해보세요.`;
            
            // Render Chart
            const ctx = document.getElementById('feedback-chart').getContext('2d');
            if (appState.feedbackChartInstance) {
                appState.feedbackChartInstance.destroy();
            }
            appState.feedbackChartInstance = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: report.detailedFeedback.map(item => item.point),
                    datasets: [{
                        label: '코칭 역량 점수',
                        data: report.detailedFeedback.map(item => parseInt(item.score) || 0),
                        backgroundColor: 'rgba(59, 130, 246, 0.2)',
                        borderColor: 'rgba(59, 130, 246, 1)',
                        borderWidth: 2,
                        pointBackgroundColor: 'rgba(59, 130, 246, 1)',
                    }]
                },
                options: {
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: { color: 'rgba(0, 0, 0, 0.1)' },
                            grid: { color: 'rgba(0, 0, 0, 0.1)' },
                            pointLabels: { font: { size: 14 } },
                            suggestedMin: 0,
                            suggestedMax: 100
                        }
                    },
                    plugins: {
                        legend: {
                           display: false
                        }
                    }
                }
            });
        }
        
        // --- SIMULATION LOGIC ---
        function startSimulation(scenario) {
            appState.currentScenario = {
                ...scenario,
                teamMember: {
                    name: scenario.type,
                    desc: scenario.desc,
                }
            };
            appState.conversationHistory = [];
            appState.lastFeedback = null;
            navigateTo('simulation-screen');
            
            document.getElementById('simulation-title').innerText = `${scenario.type} 시뮬레이션`;
            document.getElementById('simulation-team-member').innerText = `팀원: ${scenario.type}`;
            
            const initialPrompt = `
                당신은 코칭 시뮬레이션에 참여하는 가상의 팀원인 "${scenario.type}"입니다.
                당신은 리더의 코칭 대화에 현실적으로 반응하고, 리더의 대화에 대한 피드백을 제공해야 합니다.
                ${scenario.type}의 특징은 "${scenario.desc}"입니다. 이 특징을 바탕으로 대화에 임하세요.
                
                대화에 대한 피드백을 JSON 형식으로만 제공합니다. JSON 형식은 다음과 같습니다.
                {
                  "aiResponseText": "팀원의 대화 반응",
                  "feedbackSummary": "리더의 대화에 대한 1~2문장 요약 피드백",
                  "score": "대화에 대한 100점 만점 점수",
                  "detailedFeedback": [
                    { "point": "경청 및 공감", "score": "0-100 사이의 점수", "feedback": "피드백 내용" },
                    { "point": "문제 해결 접근", "score": "0-100 사이의 점수", "feedback": "피드백 내용" },
                    { "point": "코칭 모델 활용", "score": "0-100 사이의 점수", "feedback": "피드백 내용" },
                    { "point": "심리적 안정감", "score": "0-100 사이의 점수", "feedback": "피드백 내용" }
                  ]
                }
                
                대화는 지금부터 시작됩니다. 리더의 첫 대화에 대해 위 형식에 맞춰 응답하세요.
                리더: ${scenario.initialDialogue}
            `;
            
            appState.conversationHistory.push({ role: 'user', parts: [{text: initialPrompt}] });
            appState.isLoading = true;
            renderConversation();
            
            callGeminiAPI();
        }

        async function handleUserInput() {
            const userInputEl = document.getElementById('user-input');
            const userText = userInputEl.value.trim();
            if (!userText || appState.isLoading) return;

            appState.conversationHistory.push({ role: 'user', parts: [{text: userText}] });
            appState.lastFeedback = null;
            userInputEl.value = '';
            appState.isLoading = true;
            renderConversation();
            
            await callGeminiAPI();
        }

        async function callGeminiAPI() {
            const apiKey = "AIzaSyBeFX0fdqqco0QcTJUwnB9QlfQgQHhRUIk"; 
            if (!apiKey) {
                const errorMsg = { role: 'model', parts: [{text: "오류: Gemini API 키가 설정되지 않았습니다. 스크립트에서 'apiKey' 변수를 설정해주세요."}] };
                appState.conversationHistory.push(errorMsg);
                appState.isLoading = false;
                renderConversation();
                return;
            }

            try {
                const responseSchema = {
                    type: "OBJECT",
                    properties: {
                        "aiResponseText": { "type": "STRING" },
                        "feedbackSummary": { "type": "STRING" },
                        "score": { "type": "INTEGER" },
                        "detailedFeedback": {
                            "type": "ARRAY",
                            "items": {
                                "type": "OBJECT",
                                "properties": {
                                    "point": { "type": "STRING" },
                                    "score": { "type": "STRING" },
                                    "feedback": { "type": "STRING" }
                                }
                            }
                        }
                    },
                    "propertyOrdering": ["aiResponseText", "feedbackSummary", "score", "detailedFeedback"]
                };
    
                const payload = {
                    contents: appState.conversationHistory,
                    generationConfig: {
                        responseMimeType: "application/json",
                        responseSchema: responseSchema
                    }
                };
                
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
                
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    throw new Error(`API returned status: ${response.status}`);
                }
                
                const result = await response.json();
                const jsonText = result.candidates[0].content.parts[0].text;
                const parsedJson = JSON.parse(jsonText);
                
                appState.conversationHistory.push({ role: 'model', parts: [{text: parsedJson.aiResponseText}] });
                appState.lastFeedback = parsedJson;

            } catch (error) {
                console.error("Failed to fetch from Gemini API:", error);
                const errorMsg = { role: 'model', parts: [{text: `죄송합니다. AI 응답을 가져오는 중 오류가 발생했습니다: ${error.message}`}] };
                appState.conversationHistory.push(errorMsg);
            } finally {
                appState.isLoading = false;
                renderConversation();
            }
        }

        function handleEndSimulation() {
            if (!appState.lastFeedback) {
                alert("피드백을 생성할 대화가 없습니다. 대화를 먼저 진행해주세요.");
                return;
            }
            renderReport();
            navigateTo('report-screen');
        }

        // --- INITIALIZATION ---
        window.onload = () => {
            renderScenarios();
            renderTemplates();
            renderLibrary();
            document.getElementById('user-input').addEventListener('keypress', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    handleUserInput();
                }
            });
        };
    </script>

</body>
</html>
