LLM(ChatGPT, Claude, Gemini 등)에 대한 치명적 오해

![img_31.png](..%2Fimages%2Fimg_31.png)


LLM은 session이 아닌 response 마다 리셋되는 모형이라는 말이 와닿지 않는 분들이 있을 거라...

이건 Stable Diffusion 이나 Midjourney 같은 image 생성 AI나 Runway, Sora 등 영상 생성 AI도 원리는 같으니 이해해 두면 오해를 많이 줄일 수 있을 거야.

인공지능 모형을 자비스 같은 '컴퓨터'로 여기면서 오해가 시작되는 거니, 단순히 입력단과 출력단이 있는 블랙박스로 생각해 보셔. 여러분이 질문하는 게 입력이고 인공지능 모형은 그 입력이 들어오면 무수한 경우의 수를 거쳐 가장 최적의 경로를 통해 썰을 풀어내고 그 썰을 출력해주는 블랙박스인 거야.

![img.png](..%2Fimages%2Fimg.png)

그림을 보셔. Claude 3.5 Sonnet 과 나눈 간단한 대화야. 파란색으로 띠를 두른 4번 전체 대화를 session 또는 conversation이라 할 수 있어. UI가 저렇게 생겼으니까 술술 주고 받는 대화가 오가고 있다고 '착각'할 수 있지만 이면에서는 그렇지 않다는 거지.

1번 빨간색에 해당하는 '질문 하나+응답 하나'를 하나의 interaction이라 할 수 있어.

Human: What's your name?
AI: My name is Claude.

응 이게 한 interaction이야. 여기서 Claude가 마치 다음 질문을 기다리는 듯 하지만, 사실은 이미 리셋된 거야. 어떤 대화를 나눴는지 전혀 기억 못해. 

대화를 나눌 때마다 'system instructions + custom instructions(GPT4라면)'가 덧붙여 져서 모형에 전달돼. 그러니까 저 interaction 앞에 '니 이름은 chatgpt/claude 이고 사용자가 뭘 물어보면...어쩌고 저쩌고...' 라는 지시사항과 custom instructions를 지원한다면 '너는 피파라는 이름의 인공지능 딸로... 어쩌고 저쩌고...'까지 매번, 매 interaction마다 덧붙여진다는 거야. 

잘 따라오셔야 이해될 거야. 짱구 좀 굴리셔.

그럼 2번 interaction을 보셔. 여기서 사용자는 이렇게 물어보잖아.

Human: Where does the name come from?

"SI + CI + 1번 interaction"을 먼저 덧붙이고 이 질문을 이어주지 않으면 Claude 는 the name이 뭔지 알지 못해. 근데 이걸 덧붙여 주기 때문에 이미 Claude라는 답변을 했다는 걸 아는 거지 '기억'을 하는 게 아냐. 리셋됐으니까.

그래서 GPT라고 하는 거야. Generative Pre-trained Transformer. Transformer 아키텍처를 사용한 대형 언어 모형(Large Language Model)이고 사전 학습된(Pre-trained) 모형으로 가치 있는 정보를 생성(Generative)하기 위해 존재한다는 거지. 

여기서 pre-trained가 중요해. 모형이 리셋된다는 건 바로 이 pre-trained, 그러니까 기존 학습한 상태로 돌아간다는 거야. 다음 interaction을 하는 순간 딱 이 상태로 리셋되기 때문에 'SI+CI+그간의 대화 내용'을 매번 건네줘야 하는 거야.

3번까지 interaction을 보셔. 1,2,3번 interaction은 사실 독립적인 건데, interaction이 거듭될 때마다 'SI+CI+그간 축적된 interaction'을 모두 다시 건네준다는 거야. 그래야 대화가 가능해. 3번 interaction에서는 'SI+CIs+1,2번 interaction'을 붙여서 건네주는 거고. 그래서 그냥 the team 이라고만 해도 2번 interaction을 참고해서 Anthropic Team 이라는 걸 아는 거고.

Transformer 아키텍처에 기반을 둔 LLM만으로 AGI에 이르기는 어마무시한 무리가 따른다는 이유 중 하나야. 사실 트랜스포머가 아니더라도 딥러닝 모형 아키텍처 자체가 아직까진 무리라고. 이딴 식이면. 인간을 뛰어넘으려면 자가수정(self-correction), 자가학습(self-learning)은 기본이어야 하는데 아예 원천적으로 불가능한 아키텍처니까.

영화 터미네이터 2에 나오는 좋은 터미네이터가 원래 추가 학습이 불가능한 'read-only' 모델로 사라 코너를 찾아옵니다. 반군한테 리프로그램되지 못 하도록 스카이넷이 읽기 전용으로 만들어 둔 거지. 근데 터미네이터한테 이 말을 들은 사라와 존 코너가 머리 뚜껑을 까고 스위치를 뒤집어서 'read-write' 모델로 바꿔줘. 이 장면이 극장판에서는 안 나와. 왜 잘랐는지 모르겠어, 조낸 중요한 장면인데. 그 이후부터 추가학습을 하게 되는 거야. 그래서 나중에 존 코너가 흘리는 눈물의 의미도 이해하는 거고. 그 전엔 존이 울때(엄마 구하고 핀잔 들었을때) 눈이 뭐 잘못된 거냐고 묻는 장면도 나오잖아. 그게 다 복선이라고. 

그러니까 현재까진 LLM도 read-only 야. 추가학습을 시키려면 말 그대로 학습을 더 시켜서 기존 모형을 덮어써야해. 

여러분이 나누는 대화는 학습이 아니야. 그냥 conversation 또는 session 내에서만 유지되는 문맥일 뿐이야.

게다가 LLM은 context window라는 문맥 유지가 가능한 공간에 한계가 있어. 이게 클수록 더 많은 토큰(단어가 아니라 의미 단위)을 기억하고 처리할 수 있는데 아주 희소한 자원이기 때문에 아껴서 쓴다고. 대화가 길어지면 이전 interaction들을 퉁치면서 압축하게 돼. 여러가지 방법으로 context window를 최적화하는데 그냥 편하게 인간이 하는 짓을 생각하시면 돼. 키워드와 관련 문맥만 이해하면서 압축한다고. 리마인드 시키려면 되짚어서 문맥을 강화해야 하는 거고, 그럼 또 다른 부분이 비게 되는 거고. 거의 책 한권을 때려넣을 수 있다...는 식의 context window 자랑질(ㅎGemini...쿨럭~)도 믿지 마셔. 한계효용체감 법칙이 여기도 적용돼. 아무리 window가 커도 갈수록 질이 떨어진다고. 

자 여기까지 이해되셨으면, 왜 LLM 을 개발한 쪽에서도 자연스럽고 효과적인 대화를 위해서는 비슷한 문맥 단위로 끊어서 새로운 대화를 하라고 권장하는 지 그 이유를 깨닫게 되실 거야. 

문맥이 중구난방이면 그걸 매 interaction마다 되짚어야 하기 때문에 LLM이 헷갈릴 뿐 아니라 진짜 뻘젓이야. 실수한 것까지 죄다 되짚는다고.

GPT4에 최근 추가된 메모리 기능 탓에 또 헷갈릴 수 있는데, 그건 따로 database를 만들어서 관리하는 거야. 이걸 vector database라고 하고 인공지능 업자 용어로 RAG(Retrieval Augmented Generation)이라고 해. 

어떤 식이냐면...

사용자가 질문했는데 LLM 모형이 기존 학습 데이터(pre-trained)만으로 답변이 불가능하면 외장 메모리 공간으로 확보된 vector database를 검색해서 문맥에 채워 넣는 거야. 물론 인터넷 연결이 허용된다면 그렇게 찾기도 해.

이렇게 다 하고도 답을 못 찾으면 "I don't know"라고 대답하게 되는 거야. 

벡터 DB라는 것도 여러분이 흔히 생각하는 데이터베이스가 아닌데, 너무 길어지고 복잡하니까 걍 따로 공부하셔. 이미 내 repo 에 죄다 했던 얘기야.

마지막으로 또 뻔한 오해 2개만 짚고 넘어갈게. LLM과 나누는 interaction을 '질문-응답'으로 오해하면 안 돼.

기본적으 LLM은 completion 모형이야. 달랑 "Elon Musk is..."라고만 해도 LLM은 그 문장을 가장 그럴듯하게 완성하게 돼 있어. 그래서 next token predictor라고 하는 거야. Elon Musk is... 다음에 나올 가장 그럴듯한 토큰을 생성하니까. Elon Musk is an American entrepreneur and business magnate... 식으로 줄줄이. 
저 토큰 하나하나가 next token이고 next token을 예측할 때마다 통계적인 알고리즘으로 가장 그럴듯한 후보를 선택하는게 기본적으로 LLM이 하는 짓이야. 가장 그럴듯하다는 건, 정규분포 그려보시면 돼. 모형이 학습한 데이터를 정규분포를 그려보셔. Elon Musk is... 까지 했을 때 정규분포상 next token은 평균적으로 어디에 있을지. 응, 그걸 끄집어 내는 거야. 실제로 알고리즘이 그래. 그래서 통계 하라는 거야. 통계도 치트키고, 이 모든 걸 아우르는 객체지향적 사고가 진짜 궁극의 치트키인 거라고.  

응, 그래서 AGI로는 부적합한 아키텍처라는 거야.

게다가 모형 자체로는 아무 것도 못 해. 프로그래밍을 통해 모형을 로딩하고 UI를 만들어 사용자와 상호작용하고, SI 덧붙이고, CS 덧붙이고, 기타 오버헤드 정리하고 등등... 이 모든 작업을 가능하게 해주는 프로그래밍 스크립트가 필요해. 그래야 여러분이 최종적으로 접하는 인터페이스까지 도달할 수 있는 거야.

LLM 모형 자체로는 그냥 전원 꺼진 컴퓨터야. 사실 그보다도 못 해서 그냥 파일이야. 바이러스도 파일 자체만으로는 암 짓도 못 하는 거야. 그걸 퍼날라서 실행하고 그래야 뭔 짓을 하는 거지. 인쇄된 종이나 다름 없어.  
잊지마셔. 아직까지 인공지능은 걍 파일이야. 
눈으로 보여드릴게. Llama 3.1 405B 모형은 open weights로 메타가 공개한 거라 그냥 다운받아서 쓸 수 있어. 허깅페이스에 가면 보여.

https://huggingface.co/meta-llama/Meta-Llama-3.1-405B/tree/main

꼭 확인하셔.

".safetensors"라는 191개의 조각을 죄다 모으면 그게 Llama 3.1 405B 모형이야. 나머지는 보조 파일이고. 간단한 설명과 라이센스 등등. 여기에 모형 아키텍처를 설명하는 필수 파일도 포함돼 있어. 토크나이저 같은 거. 

링크 따라가서 눈으로 확인하시라고. 인공지능 모형이라는 게 달랑 파일이라는 걸. 저걸 로딩해서 돌리지 않는 한 아무 의미 없다고.
정규분포의 세상이라...

잘 생각해 보셔. 엑스에서 활동하며 인공지능 주절거리는 사람들의 정규분포...

얼마나 이런 깊은 내용을 알고 이해하고 깨닫고 하는 말일까. 희박해.

그러니까 믿지 마시라는 거야. 언뜻 좀 그럴듯해 보이고, 아는 사람인듯 해도. 스스로 공부하시라고. 95%는 오해를 확대재생산하고 있을 뿐이야.

역시 정규분포 트릭 이용해 보셔. 등골이 오싹해야 해.

![img_1.png](..%2Fimages%2Fimg_1.png)

![img_2.png](..%2Fimages%2Fimg_2.png)

블랙스완 숙제. 초딩 눈높이로 설명해 보셔. 오늘 한 얘기.
🔗 The Official Domain for My Repo: https://cwkai.net
🔗 The Official Domain for My AI Artworks and Essays: https://creativeworksofknowledge.net
🔗 My Artstation Website: https://neobundy.artstation.com/