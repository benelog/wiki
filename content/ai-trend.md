## 기업 동향
* [Claude Code PM은 어떻게 일하는가 — Cat Wu가 공개한 6개월→1일 출시법](https://www.youtube.com/watch?v=XbvoUS-j2zo)
	* 'Mission means teams are willling to make sacrifices that hurt their own goals and their own KRs in service of Antrohpic's goals and KRs. And people are  very happy to make those trade-offs. An extreme examle: if Claude Code failed but Anthropic succeed, I woule be extremely happy.'
* [EP 91. 26.1Q 비즈니스 관점에서의 AI](https://www.youtube.com/watch?v=FPYOVt2B5EM) (AI Frontier, 노정석/최승준, 2026.3.21)
    * 모든 문제를 'compute 기반의 search problem'으로 전환하는 것이 AI 시대의 본질
    * RLVR(검증 가능한 보상 기반 강화학습): 검증 가능한 보상 신호가 있으면 모델은 학습 가능. 수학, 코딩, 의료, 법무로 확장 중
    * Ralph Loop와 Meta Cascading: 조직 계층(대표-임원-팀장-팀원)과 동형의 루프 구조
    * Bundle-Unbundle 프레임워크: 패러다임 전환마다 기존 서비스가 분해되고 새로운 지배자가 다시 묶는 반복 구조
    * 에이전트가 기존 앱을 대신 조작하는 상위 레이어로 작동 → 배민, 쿠팡 등 중개 플랫폼의 마진 침식 가능성 (Disintermediation)
    * **1/10x 효율화 vs 10x 신사업**: 효율화는 데이터 커넥터 + 명확한 프롬프트 + 프론티어 모델 조합(바닐라)이 최선. 신사업은 사업가적 직관과 의지가 필수
    * 4년간 모델 구축, LoRA, 소형 언어모델 등 시도 후 결론: 프론티어 모델의 capability overhang에 의존하는 것이 최적
    * AX의 목표는 '팀 지원'이 아니라 '업무 완전 제거'
    * 에이전트 시대 인재: 모호한 목표를 명확히 정의하는 능력, 사업가적 감각, 도메인 이해 기반 프롬프트 작성
    * Prompt Injection 위협 → VM 격리, 제한된 접근 권한으로 완화
    * "요트 경기처럼 바뀌는 시장 환경에 따라가야 한다" - 기술적 우월성보다 변화에 따른 방향 전환 속도가 승패를 결정
* [Claude Blue - 실리콘밸리 전체가 우울하다](https://brunch.co.kr/@hiclemi/146) (메타 엔지니어인 선배, 실리콘밸리 창업자 친구와의 대화)
    * 회사에서 Claude Code $2,000 정도 사용. Obidian, Jira, Confluece, Google workspace 연동
    * 회사 인프라 안정성이 흔들리고 있음
    * 바이브 코딩 -> 에이언트 오케스트레이션
    * 전 직군이 AI agent 활용
    * Opus 4.5, Codex 5.4가 '나를 대체할 수 있겠네'라고 느낀 분기점
    * 베이스 모델을 가진 회사는 5개 : OpenAI, Antropic, Google, xAI(Grok), Meta
    * 다음 세대에는 경영자 마인드가 더욱 중요
    * 창업가 친구의 말 '고객에게는 오늘은 팔고, 투자자에게는 10년을 팔아야 한다. 그런데 10년 후에 우리 모두 침대에 누워있을 것을 너무나도 잘 안다.'\
    * 인적 관계 형성은 여전히 대면이 중요. AI가 대체할 수 없음
* [디즈니 직원의 AI 사용량…한 명이 9일간 ‘46만 번’ 질문](https://www.donga.com/news/Inter/article/all/20260428/133828507/2)
    * 디즈니와 ESPN 직원 약 4800명이 Cursor와 Claude를 업무에 적극 활용 중이라는 AI 도입 대시보드 사례
    * 한 직원은 9일 동안 Claude에 약 46만600회 질문했고, 2억3420만 토큰을 사용한 것으로 집계
    * 같은 기간 전체 사용량은 Claude 31억 토큰, Cursor 133억 토큰으로 추산
    * 에이전트가 다른 AI 봇에게 연쇄적으로 작업을 맡기면서 토큰 사용량이 폭증하는 구조를 보여줌
    * 토큰 처리량이 높은 엔지니어를 유능하게 보는 실리콘밸리의 ‘토큰맥싱’ 트렌드 사례
* [‘라인 코딩의 종말’… 300억 달러 가치 ‘커서(Cursor)’의 소리 없는 전시상황 소식발행일](https://welaunch.kr/post/welaunch/%E2%80%98%EB%9D%BC%EC%9D%B8-%EC%BD%94%EB%94%A9%EC%9D%98-%EC%A2%85%EB%A7%90%E2%80%99%E2%80%A6-300%EC%96%B5-%EB%8B%AC%EB%9F%AC-%EA%B0%80%EC%B9%98-%E2%80%98%EC%BB%A4%EC%84%9CCursor%E2%80%99%EC%9D%98-%EC%86%8C%EB%A6%AC-%EC%97%86%EB%8A%94-%EC%A0%84%EC%8B%9C%EC%83%81%ED%99%A9)
* [Is Cursor Profitable Today?](https://mktclarity.com/blogs/news/is-cursor-profitable)
	* Investment firm **Foundamental** calculates Cursor pays **$650 million** annually to Anthropic while generating only **$500 million** in revenue, creating a **negative 30% gross margin**. 
	* 투자 회사 파운다멘탈(Foundamental)의 계산에 따르면, 커서(Cursor)는 5억 달러의 매출을 올리는 동안 앤스로픽(Anthropic)에 연간 6억 5천만 달러를 지불하여 **-30%**의 매출총이익률을 기록하고 있습니다.
* [Cursor Goes To War For AI Coding Dominance](https://www.forbes.com/sites/annatong/2026/03/05/cursor-goes-to-war-for-ai-coding-dominance/)
	* **보조금 논란:** 앤스로픽이 자사 에이전트 서비스(예: 월 200달러 플랜)를 실제 컴퓨팅 비용보다 훨씬 저렴하게 제공함으로써 시장 점유율을 확보하려 한다는 '보조금 전략' 의혹
* [AI Tooling for Software Engineers in 2026(2026.3.4)](https://newsletter.pragmaticengineer.com/p/ai-tooling-2026)
* [Feb 2026: Claude share has surged to ~70%.](https://x.com/Yuchenj_UW/status/2028974344710606905)
* https://opentools.ai/news/george-noble-sounds-alarm-on-openai-falling-apart-in-real-time
* [플랫폼이 되고 싶었던 ChatGPT, 위기에 빠지다](https://outstanding.kr/chatgptcrisis20260128)
