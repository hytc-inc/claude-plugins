# HYTC 사내 Claude 플러그인 마켓플레이스

사내 Claude Desktop에 노출되는 플러그인 목록이다.
각 PC의 정책(`HKCU\SOFTWARE\Policies\Claude`)에 이 저장소가 등록돼 있고,
앱이 주기적으로 다시 클론한다.

**플러그인을 추가·수정하려면 이 저장소만 고치면 된다. 설정 파일(.reg) 재배포는 필요 없다.**

---

## 구조

```
.claude-plugin/marketplace.json      플러그인 목록(카탈로그)
plugins/<이름>/.claude-plugin/plugin.json   각 플러그인 정의
```

---

## ⚠️ 반드시 로컬 경로로 참조할 것

`marketplace.json` 의 `source` 는 **이 저장소 안의 경로**여야 한다.

```json
"source": "./plugins/korean-law"
```

외부 저장소를 가리키는 방식은 **Claude Desktop 에서 목록에 뜨지 않는다.**
문서상 유효한 스키마지만 실제로 동작하지 않았다(2026-08-14 확인).

```json
// 이렇게 하면 목록이 빈 채로 나온다
"source": { "source": "github", "repo": "owner/repo" }
```

---

## 플러그인 추가 방법

**1. 플러그인 정의를 만든다** — `plugins/<이름>/.claude-plugin/plugin.json`

```json
{
  "name": "플러그인이름",
  "version": "1.0.0",
  "description": "무엇을 하는지",
  "mcpServers": {
    "서버이름": {
      "command": "npx",
      "args": ["-y", "패키지명@latest"],
      "env": { "API_KEY": "${user_config.api_key}" }
    }
  },
  "userConfig": {
    "api_key": {
      "type": "string",
      "title": "화면에 표시될 이름",
      "description": "발급 방법 안내",
      "sensitive": true
    }
  }
}
```

`userConfig` 를 쓰면 **설치할 때 앱이 사용자에게 키를 물어본다.**
관리자가 키를 배포할 필요가 없고, 각자 본인 키를 쓰므로 공유 키 약관 문제도 없다.

원격 MCP 서버라면 `mcpServers` 대신 URL 방식을 쓴다.

**2. 카탈로그에 등록한다** — `.claude-plugin/marketplace.json` 의 `plugins` 배열에 추가

```json
{
  "name": "플러그인이름",
  "source": "./plugins/플러그인이름",
  "description": "사전 준비가 필요하면 함께 적는다",
  "version": "1.0.0",
  "category": "분류",
  "tags": ["태그"]
}
```

**3. 커밋한다.** 끝. 사용자는 Claude 재시작 후 디렉터리에서 설치한다.

---

## 현재 등록된 플러그인

| 이름 | 용도 | 사전 준비 |
|---|---|---|
| `korean-law` | 법령·판례·조례·해석례 조회 | Node.js 20+, 법제처 Open API 인증키(무료, 설치 중 입력) |

---

## 검토 기준

플러그인은 사용자 PC에서 코드를 실행한다. 추가 전에 확인할 것:

- 출처가 신뢰할 만한가 (저장소 활동, 유지보수 여부)
- 어떤 데이터를 외부로 보내는가 — 사내 기밀을 다루는 도구는 특히 주의
- 인증키가 필요하면 개인별 발급인가 (공유 키는 약관 위반 소지)
- 라이선스

버전을 `@latest` 로 두면 원저작자 배포를 그대로 따라간다.
안정성이 중요하면 특정 버전으로 고정할 것.

---

## 사용자 안내

설치 방법은 `법령검색MCP_사용자안내.md` 참조.
사내 Bedrock 운영 전반은 `Bedrock_운영관리_정리.md` 참조.
