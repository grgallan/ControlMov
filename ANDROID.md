# App Android (Capacitor)

Esse projeto também gera um app Android de verdade (instalável, ícone
próprio), usando o [Capacitor](https://capacitorjs.com/). Não é um app
separado com código duplicado — é uma "casca" nativa que abre o site já
publicado (`https://grgallan.github.io/ControlMov/`). Ou seja:

- **O site continua funcionando exatamente como hoje**, sem nenhuma mudança,
  pra quem acessa pelo navegador.
- **O app Android reflete qualquer atualização do site automaticamente**,
  na próxima vez que for aberto — não precisa gerar um novo `.apk` quando o
  `app.js`/`index.html` mudam.
- O backend (Supabase) é o mesmo do teaatend, sem nenhuma alteração — o app
  só faz as mesmas chamadas que o site já faz.

## O que já foi feito

- `package.json` + Capacitor instalado (`@capacitor/core`, `@capacitor/cli`,
  `@capacitor/android`).
- `capacitor.config.json` configurado com `appId: com.controlmov.app`,
  `appName: ControlMov`, e `server.url` apontando pro site ao vivo.
- Pasta `android/` com o projeto nativo completo (gerado por `npx cap add android`).
- Ícone e splash screen **provisórios** — por enquanto a mesma logo do
  teaatend, só pra ter algo instalável — ver
  [Trocar o ícone](#trocar-o-ícone-pela-logo-de-verdade) pra usar uma logo
  própria do ControlMov quando tiverem uma.
- `scripts/sync-www.js`: copia `index.html`, `app.js`, `manifest.json`,
  `sw.js` e `icons/` pra dentro de `www/`, que é o que o Capacitor empacota
  no app.
- Workflow de CI (`.github/workflows/android-build.yml`) que compila um
  `.apk` de debug automaticamente e disponibiliza pra download.

## Gerar o APK (GitHub Actions, sem instalar nada)

1. Repositório → aba **Actions** → workflow **"Build Android APK"** →
   **Run workflow**.
2. Espere terminar (uns 3-5 minutos) e baixe o arquivo `controlmov-debug-apk`
   nos artefatos da execução — é o `.apk`, pronto pra instalar num Android
   (ative "Instalar de fontes desconhecidas" ou envie por um app de
   mensagens/e-mail pra si mesmo).

## Android Studio local (necessário pra publicar na Play Store)

1. Instale o [Android Studio](https://developer.android.com/studio).
2. `npm install` na raiz do projeto.
3. `npm run cap:sync` (atualiza `www/` e sincroniza com o projeto Android).
4. `npx cap open android` — abre o projeto no Android Studio.
5. Rode num emulador ou celular conectado (▶ Run), ou gere um `.apk`/`.aab`
   assinado por **Build → Generate Signed Bundle / APK** (necessário pra
   publicar na Play Store — guarde a keystore gerada em local seguro).

## Trocar o ícone pela logo de verdade

1. Substituam `assets/icon.png` (1024×1024, sem cantos arredondados) e,
   se quiserem uma splash screen personalizada, `assets/splash.png`
   (2732×2732, logo centralizada).
2. Rode:
   ```bash
   npm install
   npx capacitor-assets generate --android
   ```
3. Isso regenera todos os tamanhos/densidades automaticamente dentro de
   `android/app/src/main/res/`. Commitem o resultado.

## Estrutura

```
capacitor.config.json   → configuração do Capacitor (appId, appName, URL do site)
scripts/sync-www.js     → copia os arquivos públicos do site pra www/
assets/                 → ícone/splash "fonte" (edite aqui, não em android/res/)
android/                → projeto nativo Android (gerado pelo Capacitor)
www/                    → gerado por "npm run sync-www" — não editar direto, nem versionar
```
