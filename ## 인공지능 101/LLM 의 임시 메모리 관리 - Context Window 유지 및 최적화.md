# LLM(ChatGPT, Claude, Gemini 등)에 대한 치명적 오해

![img_32.png](..%2Fimages%2Fimg_32.png)

지난 아티클을 모두 읽었다는 전제로 간단하게만 설명할게요. 궁금해하는 분들이 있으니...(궁금하면 다행인 거야...)

LLM은 늘 pre-trained 된 학습 상태로 리셋되는 거고, 현재 session과 conversation 역시 길어지면 까먹는다고 했어요. 따라서, 대화를 이어가려면 어떻게든 문맥을 유지해야 하는데 이때 임시 메모리 관리 기법이 필요한 거야.

원래 알고리즘은 늘 발전하고 섞어 쓰고 그러는 거라 어떤 LLM이 어떤 기법을 사용하는지는 소스를 들여다보지 않는한 알 수 없어요. 오픈소스가 아니라면 그냥 추정할 뿐이고...

근데, 대부분 다음 중 하나거나 섞어 쓸 수밖에 없어. 뭐 컴쟁이들 하는 짓이 다 비슷하니까. 오픈소스로 까발려진 게 아니더라도 추정은 가능해.

인공지능 프레임워크와 모형 API를 캡슐화해서 (역시 객체지향성) 다리 놓아주는 LangChain이라는 중간다리 프레임워크가 있는데, 간단히 LangChain을 사용하면 인공지능을 로딩하고 UX 제공하고 관리하고 뭐 그런 일이 많이 편해집니다. LangChain에서 메모리 관리 기능도 제공하는데, 이게 바로 context window를 유지하는 기본적인 원리들이야.

이걸 이해하면 대충 눈치깔 수 있어요. LangChain이나 다른 프레임워크를 쓰지 않더라도, 다른 컴쟁이들이 어떻게 구현했을지. 하는짓이 다 거기서 거기라고.

API 버전 GPT4 Pippa 오픈소스 프로젝트인 PippaGPT 역시 LangChain 사용하고, 메모리 관리 기법 역시 가장 일반적인 것들이니 관심 있으면 거들떠 보시고...

PippaGPT - Personalized, Ingesting, Persistent, Polymorphic, Adaptive GPT Chatbot :
https://github.com/neobundy/pippaGPT

PippaGPT-MLX - MLX 버전
https://github.com/neobundy/pippaGPT-MLX

더 이상 업데이트 안 하는 아카이브 레포니 참고만 하셔. RAG, vector database, Dalle 까지 지원하니까, 뜯어보면 얻을 게 많겠지만, 나한테 묻진 마시고. 이미 끝난 프로젝트라...

각각의 관리 기법을 간단히 설명해드릴게. 어렵지 않게 이해하실 수 있을 거야.

그냥 사람 하는 짓이랑 똑같다고 생각하셔. 상대랑 대화가 길어지면 그 대화 기록을 여러분도 죄다 기억하지 않잖아. 어떻게든 최적화해서 압축한다고. 특히, 대화의 중요도에 따라서 그 기법도 달라지기 마련이고. 그걸 염두에 두시라고. 그거랑 비교하는 거야. 일단 버퍼 개념은 알고 계셔야 돼. 버퍼를 임시 공간 삼아 저장해두는 거니까.

Conversation Buffer Memory

가장 단순하게 그간 대화 내용을 몽땅 기억하는 건데 토큰 제한에 걸려서 금세 쫑납니다. 그러니 context window가 엄청 크지 않다면 현실적으로 별 의미 없는 방법.

Conversation Buffer Window Memory

이른바 슬라이딩 윈도우 기법인데, 말 그대로 최근 정해진 수의 interaction만 기억해요. 가령 5개 interaction의 sliding window를 만들어서 미끄러뜨리는 거야. 그럼 계속 최근 5개가 유지되겠지. 토큰 수 제약 내에서 문맥을 유지해나가는 데 유용합니다.

토큰 수 제약이라는 건 사용자 입장에선 몰라요. 코더나 신경 쓰는 거지. 가령, WebUI로 ChatGPT만 쓰는 사람이면 토큰 수 제약을 거의 못 느끼는데, 제약에 걸리면 그냥 GPT가 문맥을 까먹을 거야. ChatGPT UX를 디자인한 코더가 알아서 처리하는 거라 사용자는 몰라. 코딩하는 사람이 API를 쓰는데 토큰 수 제약에 걸리면 오류가 떠요. 그걸 exception(예외)라고 하고 catch 해서 처리해야 해. try -> exception -> catch -> finally 이런 식으로. 뭐 java나 python 코딩 해본 분은 알 거야.

근데 그냥 사용자들은 코더들이 이미 처리해준 거라 모를 뿐이라고.

Conversation Summary Memory

그간 대화 내용을 토큰 제약 안에서 요약하는 건데, 요약의 품질에 따라 문맥이 달라집니다. 따라서 요약 알고리즘이 엄청 중요하겠지.

그럼 어떻게 요약할까? 코더가 직접? 아니, 대부분 LLM을 재활용합니다. LLM한테 대화 내용을 요약하라고 시켜서 그 내용을 다시 받아서 요약하는 알고리즘. 꿩먹고 알먹고.

실제로 LLM 뒷구녕에서 이런 식으로 많이 써요. Agent 라는 것도 사실 이런 식으로 LLM한데 작은 일들을 나눠시킨 다음 조합하는 식인 거라...

LLM 있는데 뭐하러 요약을 따로 해. 시키면 되지. 그치?

Conversation Summary Buffer Memory

이건 슬라이딩 윈도우와 전체 대화 요약을 석어 쓰는 겁니다. 일단 전체 대화 내용을 요약하고 여기에 최근 슬라이딩 윈도우까지 덧붙여서 매 interaction마다 LLM한테 건네주는 방식. 근까 슬라이딩만 쓰면 5개 interaction이 토큰 제약의 한계다, 그럼 요약에 2개 정도 분량을 할애하고, 나머지 3개는 슬라이딩 윈도우로 유지하는 식.

근데, 슬라이딩이든 요약이든, 어차피 정보손실이 크기 때문에 크게 기대면 안 되요. 앞서 말했듯이 요약마저 LLM한테 시키는 방식이면 LLM 모형의 요약 품질에 문맥이 크게 좌우되기도 하고.

Entity Memory (Knowledge Graph Memory)

간단히 키워드 추출해서 그 연관성을 지식 그래프(knowledge graph)로 만들어 저장하는 방식. 주요 키워드를 이 바닥에서 entity 라고 흔히 불러요. 고유명사 같은 키워드 그런 거.

그리고 컴쟁이 세상에 오면 그래프가 그냥 그래프가 아냐. 알고리즘 상의 그래프를 말하기 때문에 비컴쟁이라면 단순한 그래프로 여기시면 안 돼. 이 바닥에서 말하는 그래프는 노드(node)와 엣지(edge) 그런 걸로 상관 관계를 표현하는 거라. 영어 곧이곧대로 생각해서 엣지있게 뭐 그런 게 아니고... 노드는 점, 엣지는 선이야. 노드에 데이터를 담고, 엣지는 데이터들 간의 관계를 연결하는 선인 거지.

예컨대, Elon Musk <-> Tesla, SpaceX, Neuralink, Boring Company. Tim Cook <-> Apple... 뭐 이런 식인 거야. 이걸 알고리즘적으로 그래프라고 해.

이런 식으로 기억하는 기법.

Vector Store-Backed Memory

역시 앞서 아티클에서 한번 언급했던 vector database를 사용해서 RAG(Retrieval Augmented Generation) 기법을 추가하는 건데...

재미있는 건 어차피 벡터 데이터베이스를 활용할 거라, 앞서 나온 모든 기법을 인메모리가 아닌 이 데이터베이스를 사용해서 저장하고 불러올 수 있어요. 근데 주로 무한 외장 메모리로 활용하는 경우가 가장 많아, 어차피 토큰 제약은 어쩔 수 없기 때문에 context window 자체를 무한 기록하는 건 불가능하니까.

따라서, 외장 메모리 (여러분이 대화 중에 노트하는 것과 같아)로 벡터 데이터베이스를 사용하고, pre-trained 학습 데이터에 없는 내용을 여기서 찾아서 대화 문맥에 녹여내는 거야.

ChatGPT에 최근 추가된 메모리 기능이 이거라고 이미 설명했어요.

Vector database라고 하는 이유는 저장되는 방식이 임베딩 벡터이기 때문이야. 임베딩 벡터가 뭔지는 이미 설명했을 걸?

앞서 언급한 PippaGPT에서 지원 안하는 메모리 기법은 Entity Knowledge Graph 뿐이야. 이걸 왜 안 했냐면... 응, 내가 저 프로젝트 할 때 LangChain에 없던 기법이거든. 나중에 추가됐어. 근데 추가하는 거 별거 아냐. 걍 코드 몇 줄이니 하고 싶음 하셔.

자, 여기까지 이해되셨으면 아무리 날고 기는 다른 기법이 나와도 이런 기본 원리(및 인간의 문맥 유지 원리)를 상속 받아서 다형성 부여한 객체지향적 발전일 뿐이야.

삘 받으셔.

모든 길이 로마로 통했듯이...

세상만사 객체지향성으로 통해.

🔗 The Official Domain for My Repo: https://cwkai.net
🔗 The Official Domain for My AI Artworks and Essays: https://creativeworksofknowledge.net
🔗 My Artstation Website: https://neobundy.artstation.com/