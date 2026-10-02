# app-link

app.aihavit.com — 앱 다운로드 링크 한 장.

- **모바일:** AppsFlyer OneLink(`havit.onelink.me/crNQ`)로 보내 설치를 귀속시킵니다. 스토어 직링크를 쓰면 설치가 오가닉으로 잡혀 귀속이 끊깁니다.
- **PC:** 이동하지 않고 QR코드와 스토어 뱃지를 보여줍니다. QR에는 현재 주소를 그대로 담아서, 폰으로 찍어도 귀속 정보가 유지됩니다.
- **언어:** www.aihavit.com과 같은 34개 언어 중 기기 언어(`navigator.languages`)에 맞는 것을 고르고, 없으면 영어로 나옵니다. 아랍어·히브리어는 오른쪽→왼쪽(RTL)으로 표시합니다. OG는 크롤러가 한 번 읽고 끝나서 기기별로 바꿀 수 없어, 홈페이지 영문 OG와 같은 문구를 씁니다.

## 링크 규칙 (위에서 걸리면 아래는 보지 않음)
| 들어온 주소 | 보내는 OneLink |
|---|---|
| `pid`가 이미 붙어 있음 (impact 등 파트너 링크) | 받은 쿼리를 **한 글자도 바꾸지 않고** 그대로 |
| `irclickid` (impact 딥링크) | `pid=impactradius_int&clickid={irclickid}` |
| `oppref` (ChatGPT 광고 클릭 ID) | `pid=openai_int&clickid={oppref}` |
| `utm_source=…` | `pid={utm_source}` (facebook·google·chatgpt 등은 `*_web` / `openai_int`로 정리) |
| `gclid` / `fbclid` / `ttclid`만 있음 | `pid=google_web` / `meta_web` / `tiktok_web` |
| 아무것도 없음 | `pid=website&c=app_link` |

utm 값은 AppsFlyer 칸으로 옮겨 넣습니다(`c`=utm_campaign, `af_channel`=utm_medium, `af_ad`=utm_content, `af_keywords`=utm_term). 그 외 받은 파라미터(파트너 값, 클릭 ID)는 버리지 않고 그대로 실어 보냅니다.

## 확인
주소 뒤에 `?_debug=1`을 붙이면 이동하지 않고, 만들어진 목적지 URL을 화면에 보여줍니다.
