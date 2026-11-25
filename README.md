# ControlMov

App separado, só pro módulo de Movimentos (integração com TOTVS RM): cadastro
de Tipos de Movimentação, criação/edição/envio de Movimentações, "Imprimir"
e o construtor de Layout da Impressão.

## Como funciona

Esse app usa o **mesmo backend (Supabase) e o mesmo login/contas** do
[teaatend](https://github.com/grgallan/teaatend) — não é um sistema
separado por baixo, é uma segunda "porta de entrada" pro mesmo banco de
dados, restrita a Movimentos.

`index.html`/`app.js`/`manifest.json`/`sw.js` são cópias dos arquivos do
`teaatend` (mesmo `CONFIG.API_URL`/`ANON_KEY` em `app.js`) — nenhuma lógica
foi removida ou reescrita. A única diferença é um pequeno script no fim do
`index.html`, depois de `<script src="app.js">`, que troca a função
`entrarNoApp` por uma versão que, depois de rodar a original (que já aplica
a visibilidade de menu por perfil de acesso), esconde todo menu que não
seja "Movimentos" e já abre direto nessa tela.

**Importante**: isso é separação de produto/interface, não uma trava de
segurança adicional — os dados continuam protegidos pelas mesmas permissões
já checadas no backend (`podeAgir`), do jeito que sempre foram no sistema
principal. Uma conta sem permissão em `movimentos.movimentacoes` ou
`movimentos.parametros` continua recebendo erro do backend ao tentar usar
essas telas, independente desse app.

Cadastro de Clientes (onde fica a URL/usuário/senha do RM de cada cliente)
continua só no `teaatend` — esse app lista e usa os Clientes já
cadastrados lá, não tem tela própria pra criar/editar Cliente.

## Atualizar depois de uma mudança no teaatend

Sempre que `index.html`/`app.js`/`manifest.json`/`sw.js` do `teaatend`
mudarem, repita:

1. Copie os 4 arquivos de lá pra cá.
2. Reaplique as mudanças de branding (`<title>`, `loginEmpresaNome`,
   `.sistema`, `<h1>` do login) e o `manifest.json`.
3. Reaplique o pequeno `<script>` de override do `entrarNoApp` no fim do
   `index.html` (antes de `</body>`).
4. Renomeie a constante `CACHE` em `sw.js` se quiser forçar os navegadores
   a buscar a versão nova (opcional — o `sw.js` já é "network first").

## Deploy

Publica como site estático (GitHub Pages) — igual ao `teaatend`. Depois de
criado, precisa habilitar manualmente em **Settings → Pages** (Source:
Deploy from a branch, branch `main`, pasta `/`).
