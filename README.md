# HYTC 사내 Claude 플러그인 마켓플레이스

사내 Claude Desktop에 노출되는 플러그인 목록이다.
각 PC의 정책(`HKCU\SOFTWARE\Policies\Claude`)에 이 저장소가 등록돼 있고,
앱이 주기적으로 다시 클론한다.

**플러그인을 추가·수정하려면 `.claude-plugin/marketplace.json` 만 고치면 된다.
설정 파일(.reg) 재배포는 필요 없다.**

---

## 플러그인 추가 방법

`.claude-plugin/marketplace.json` 의 `plugins` 배열에 항목을 넣고 커밋한다.

```json
{
  "name": "플러그인이름",
  "source": { "source": "github", "repo": "owner/repo" },
  "description": "무엇을 하는지, 사전 준비가 필요하면 함께 적는다",
  "version": "1.0.0",
  "author": { "name": "제작자" },
  "homepage": "https://github.com/owner/repo"
}
```

`source` 는 외부 저장소를 그대로 가리켜도 된다. 이 저장소는 **큐레이션 목록** 역할만 한다.

사용자는 Claude에서 마켓플레이스 목록을 보고 필요한 것만 설치한다(설치 모드: 수동).

---

## 현재 등록된 플러그인

| 이름 | 용도 | 사전 준비 |
|---|---|---|
| `korean-law` | 법령·판례·조례·해석례 조회 | Node.js, 법제처 Open API 인증키(무료) |

---

## 검토 기준

플러그인은 사용자 PC에서 코드를 실행한다. 추가 전에 확인할 것:

- 출처가 신뢰할 만한가 (저장소 활동, 유지보수 여부)
- 어떤 데이터를 외부로 보내는가 — 사내 기밀을 다루는 도구는 특히 주의
- 인증키가 필요하면 개인별 발급인가 (공유 키는 약관 위반 소지)
- 라이선스

---

## 관련 문서

사내 Bedrock 운영 전반은 `Bedrock_운영관리_정리.md` 참조.
