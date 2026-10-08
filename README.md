# SuperStory Web (superstory.com)

정적 단일 파일 사이트. `public/index.html` 하나에 CSS · 데이터 · 뷰 · 이미지(data URI)가 모두 들어 있고, 해시 라우터(`#/about/history`)로 화면을 전환한다. 빌드 단계가 없다.

## 폴더

```
superstory-web/
├─ public/
│  ├─ index.html      ← 사이트 본체 (시안 v15)
│  ├─ favicon.png     32px 심볼
│  ├─ icon-180.png    apple-touch-icon
│  ├─ icon-512.png    PWA / 안드로이드
│  ├─ og.png          1200×630 공유 썸네일 (레드 BI on ink)
│  ├─ robots.txt
│  └─ sitemap.xml
├─ vercel.json        리다이렉트 · 리라이트 · 캐시 헤더
├─ package.json       dev / deploy 스크립트
└─ README.md
```

## 배포 (둘 중 하나)

**A. Vercel CLI**
```
npm i -g vercel
cd superstory-web
vercel            # 첫 배포: 프로젝트 생성 (Framework Preset: Other, Output Directory: public)
vercel --prod     # 프로덕션
```

**B. GitHub 연동**
1. 이 폴더를 저장소 루트로 푸시
2. Vercel → Add New Project → 저장소 선택
3. Framework Preset `Other`, Build Command 비움, Output Directory `public`
4. Deploy

## 도메인

Vercel 프로젝트 → Settings → Domains 에 아래 네 개를 추가한다.

| 도메인 | 역할 | DNS |
|---|---|---|
| `superstory.com` | 기본 | A `76.76.21.21` |
| `www.superstory.com` | → superstory.com 301 (vercel.json) | CNAME `cname.vercel-dns.com` |
| `storynmics.com` | → superstory.com/#/about 301 (vercel.json) | A `76.76.21.21` |
| `www.storynmics.com` | 위와 동일 | CNAME `cname.vercel-dns.com` |

storynmics 리다이렉트는 도메인을 이 프로젝트에 붙이는 순간부터 동작한다. 아직 전환 시점이 정해지지 않았으면 `superstory.com` 두 개만 먼저 붙이면 된다.

## vercel.json 요약

- `rewrites`: 정적 파일 외 모든 경로 → `/index.html` (배포 후 `/about/history` 같은 path 라우팅으로 바꿔도 그대로 동작)
- `redirects`: www → apex, storynmics → superstory /about
- `headers`: index.html 은 재검증(must-revalidate), 이미지는 1년 캐시

## 로컬 확인

```
npm run dev   # http://localhost:3000
```

## 배포 후 체크리스트

- [ ] `/#/about/location` 구글 지도 iframe 표시 (아티팩트에서는 차단됐던 항목)
- [ ] `/#/about/download` 파일 요청 버튼 → 실제 ZIP · PDF 링크로 교체 (파일 확보 후)
- [ ] `/#/playtoon/demo` 데모 14종 링크를 기존 데모 플레이어 URL로 연결
- [ ] `/#/contact` mailto → 서버 접수(폼 API)로 전환
- [ ] Pretendard(jsDelivr) · Google Fonts 로드 확인, 이후 woff2 자체 호스팅 전환
- [ ] 개인정보 처리방침 시행일 확정
- [ ] Search Console 에 sitemap 제출

## 갱신 방법

콘텐츠 수정은 `public/index.html` 안의 `site2.js` 영역(데이터: NAV · IPS · NEWS · HIST · BIZ · DEMOS · CTYPES)과 `site3.js` 영역(뷰)에서 한다. 수정 후 `vercel --prod` 또는 git push.
