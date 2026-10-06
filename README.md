# app-link

app.aihavit.com — 앱 다운로드 링크 한 장.

- **모바일:** AppsFlyer OneLink(`havit.onelink.me/crNQ`)로 보내 설치를 귀속시킵니다. 스토어 직링크를 쓰면 설치가 오가닉으로 잡혀 귀속이 끊깁니다.
- **PC:** 이동하지 않고 QR코드와 스토어 뱃지를 보여줍니다. QR에는 폰에서 만들 OneLink 주소를 그대로 담아서, 폰으로 찍으면 이 페이지를 거치지 않고 바로 스토어로 가며 귀속 정보도 같습니다. 단 `af_adset`은 `desktop_qr`로 바꿔 PC→QR 경유를 따로 집계합니다(파트너 링크처럼 `pid`가 붙어 온 경우와 `af_adset`을 직접 받은 경우는 그대로).
- **언어:** www.aihavit.com과 같은 34개 언어 중 기기 언어(`navigator.languages`)에 맞는 것을 고르고, 없으면 영어로 나옵니다. 아랍어·히브리어는 오른쪽→왼쪽(RTL)으로 표시합니다. OG는 크롤러가 한 번 읽고 끝나서 기기별로 바꿀 수 없어, 홈페이지 영문 OG와 같은 문구를 씁니다.

## 링크 규칙 (위에서 걸리면 아래는 보지 않음)
| 들어온 주소 | 보내는 OneLink |
|---|---|
| `pid`가 이미 붙어 있음 (impact 등 파트너 링크) | 받은 쿼리를 **한 글자도 바꾸지 않고** 그대로 |
| `irclickid` (impact 딥링크) | `pid=impactradius_int&clickid={irclickid}` |
| `oppref` (ChatGPT 광고 클릭 ID) | `pid=openai_int&clickid={oppref}` |
| `utm_source=…` | `pid={utm_source}` (facebook·google·chatgpt 등은 `*_web` / `openai_int`로 정리) |
| `gclid`·`gbraid`·`wbraid` / `ttclid` (광고 클릭에만 붙음) | `pid=google_web` / `tiktok_web` |
| `ch=…` (우리가 직접 만든 링크) | `pid={ch}` 소문자로 **그대로** (`*_web`으로 바꾸지 않음) |
| `fbclid` (페북·인스타가 광고 아닌 링크에도 자동으로 붙임) | `pid=meta_web` |
| 아무것도 없음 | `pid=website&c=app_link` |

utm 값은 AppsFlyer 칸으로 옮겨 넣습니다(`c`=utm_campaign, `af_channel`=utm_medium, `af_ad`=utm_content, `af_keywords`=utm_term). Google Ads 가 붙이는 `keyword`(최종 URL 접미사 `{keyword}`)가 있으면 utm_term 보다 먼저 `af_keywords` 로 씁니다. 그 외 받은 파라미터(파트너 값, 클릭 ID)는 버리지 않고 그대로 실어 보냅니다.

### `ch`로 매체 이름 붙이기
링크를 직접 만들 때는 `ch` 하나만 붙이면 그 값이 AppsFlyer Media source 가 됩니다. 캠페인·채널이 필요하면 `c`·`utm_campaign`, `utm_medium` 을 같이 쓰면 됩니다.

| 만든 링크 | Media source | 그 외 |
|---|---|---|
| `app.aihavit.com/?ch=insta_bio` | `insta_bio` | `c=app_link` |
| `app.aihavit.com/?ch=instagram` | `instagram` | `utm_source=instagram` 이면 `meta_web` 이 되는 것과 다름 |
| `app.aihavit.com/?ch=kakao_ch&c=oct_event&utm_medium=chat` | `kakao_ch` | `c=oct_event`, `af_channel=chat` |

- `utm_source` 가 같이 있으면 `utm_source` 가 이깁니다. 서버 랜딩(`/qr/`·추천인 등)이 매체를 `utm_source` 로 정해 보내고, 광고 플랫폼도 `utm_source` 를 덧붙이기 때문입니다.
- 구글·틱톡 광고 클릭 ID(`gclid`·`ttclid` 등)도 `ch` 를 이깁니다. `fbclid` 는 페북·인스타가 일반 게시물·프로필 링크에도 붙여서 `ch` 가 이깁니다 — 그래서 메타 광고에 `ch` 링크를 쓰면 `ch` 로 잡히니, 메타 광고에는 `utm_source` 를 붙이세요.
- 오타도 그대로 다른 매체가 됩니다(`insta_bio` ≠ `insat_bio`). 쓰는 값을 팀에서 정해 두세요.
- `_int` 로 끝나는 이름은 AppsFlyer 연동 파트너 전용이라 쓰지 않습니다. `meta_web`·`google_web` 같은 기존 유료 매체 이름도 피하세요.

## 확인
주소 뒤에 `?_debug=1`을 붙이면 이동하지 않고, 만들어진 목적지 URL을 화면에 보여줍니다.
