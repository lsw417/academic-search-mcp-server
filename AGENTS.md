# academic-search-mcp-server — 작업 가이드

`afrise/academic-search-mcp-server` 포크(**public**). Semantic Scholar + Crossref 논문 검색 MCP.
Claude Code user 스코프에 `academic-search`로 등록돼 있고(`~/.claude.json`, `uv run server.py`), `literature-search` 스킬이 이 서버를 쓴다.

## upstream 대비 로컬 변경 (2커밋, 브랜치 `master`)

- `S2_API_KEY` 환경변수가 있으면 Semantic Scholar 요청에만 `x-api-key`를 붙인다(전용 한도). 없으면 결과 끝에 "shared rate pool" 표시.
- Semantic Scholar 실패 시 사유를 결과에 명시한다 — 조용한 누락 방지.

## 지켜야 할 것

- **키는 이 리포에 절대 넣지 않는다.** 이 리포는 public이다. `S2_API_KEY`의 유일한 위치는 `~/.claude.json`의 `mcpServers.academic-search.env`. `.env`·`launch.json`·`smithery.yaml`에 키를 쓰지 않는다.
- 도구 3개(`search_papers` / `fetch_paper_details` / `search_by_topic`)의 이름·시그니처는 유지한다 — 스킬 프롬프트가 의존한다.
- upstream을 당겨올 때는 위 로컬 커밋 2개를 그 위에 리베이스한다.
- Semantic Scholar 무인증 풀은 자주 429가 난다. 결과가 비면 키 미설정인지 먼저 본다(결과 문자열에 표시됨).

## 배경

메모리 `project_research_skills`(어떤 검색 엔진이 실제로 있는지), llm-wiki `[[academic-search-mcp-server]]`.
