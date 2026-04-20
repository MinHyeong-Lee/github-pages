# My Wedding Invitation

Vue 3 + Vite로 만든 모바일 청첩장 프로젝트입니다.

## Development

```bash
pnpm install
pnpm run dev
```

## Deploy

이 프로젝트는 `private GitHub repository + Vercel` 연동을 기준으로 설정되어 있습니다.

1. GitHub 저장소를 `private`으로 유지합니다.
2. Vercel에서 해당 저장소를 import 합니다.
3. Vercel이 Vite 프로젝트를 자동 감지해 빌드합니다.
4. 커스텀 도메인을 쓰는 경우에도 별도 `base` 수정 없이 그대로 배포할 수 있습니다.

CLI를 사용할 경우:

```bash
npx vercel
npx vercel --prod
```

`package.json`에도 같은 흐름의 스크립트가 들어 있습니다.
