# chat-agent-mcp

일상 브리핑(날씨, 뉴스, 프로야구 순위, 일정, 명언)을 제공하는 도구 모음을 MCP(Model Context Protocol) 서버로 노출하는 예제 프로젝트입니다. 기반으로 동작하며, HTTP 스트리밍(streamable-http) 방식으로 `/mcp` 엔드포인트를 통해 클라이언트(LLM 에이전트 등)와 통신합니다.

## 제공 도구 (Tools)

| 도구 | 설명 |
| --- | --- |
| `scrape_page_text(url)` | 주어진 URL의 웹페이지에서 본문 텍스트를 추출합니다. |
| `get_weather(city_name)` | 도시 이름으로 좌표를 조회한 뒤 Open-Meteo API로 현재 날씨를 가져옵니다. |
| `get_news_headlines()` | 구글 뉴스 RSS 피드에서 최신 헤드라인과 링크를 가져옵니다. |
| `get_kbo_rank()` | 한국 프로야구(KBO) 팀 순위를 조회합니다. |
| `today_schedule()` | 하드코딩된 오늘의 일정을 반환합니다 (실제 캘린더 연동 없음). |
| `daily_quote()` | LLM을 이용해 오늘의 명언을 생성합니다. |
| `brief_today()` | 위 도구들을 순서대로 호출하도록 에이전트에게 지시하여 종합 브리핑을 생성합니다. |

## 요구 사항

- Python 3.10+
- OpenAI API Key (`daily_quote` 도구가 `ChatOpenAI`를 사용하므로 필요)

## 설치

```bash
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

## 환경 변수 설정

프로젝트 루트에 `.env` 파일을 만들고 OpenAI API 키를 설정합니다.

```env
OPENAI_API_KEY=sk-...
```

## 실행

```bash
python mcp_server.py
```

기본적으로 `127.0.0.1:8000/mcp` 에서 streamable-http 방식의 MCP 서버가 실행됩니다.

## 참고

- `get_weather`는 [Nominatim](https://nominatim.org/)(OpenStreetMap)으로 도시 좌표를 얻고, [Open-Meteo](https://open-meteo.com/) API로 날씨를 조회합니다 (무료, 별도 API 키 불필요).
- `today_schedule`은 실제 캘린더 연동이 아닌 하드코딩된 예시 데이터를 반환합니다. 실제 연동을 원한다면 [MCP 인증 스펙](https://modelcontextprotocol.io/specification/draft/basic/authorization)을 참고하여 구현하세요.

