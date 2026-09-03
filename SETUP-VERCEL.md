# Instância própria do card de estatísticas

O card de **estatísticas + linguagens** do README (`github-readme-stats`) roda numa
instância sua na Vercel, porque a instância pública vive caindo com erro **429/503**.

- **Projeto na Vercel:** `github-readme-stats` → domínio `github-readme-stats-nine-ashy-18.vercel.app`
- **Fork:** `Renan0204/github-readme-stats` (de `anuraghazra/github-readme-stats`)
- **Env var:** `PAT_1` = um Personal Access Token **classic** com escopo `repo` + `read:user`
- **Ambiente:** Production

## Se precisar recriar ou trocar o token

1. Gere um token classic em <https://github.com/settings/tokens> → **Generate new token (classic)**
   → escopos `repo` e `read:user` → copie o `ghp_...`.
2. Teste antes (PowerShell):
   ```powershell
   curl.exe -s -H "Authorization: token COLE_O_TOKEN" https://api.github.com/user
   ```
   Tem que voltar `"login": "renan0204"`.
3. Vercel → projeto `github-readme-stats` → **Settings → Environment Variables** →
   `PAT_1` → **Edit** → cole o valor → **Save**.
4. **Deployments** → deployment de **Production** → **⋯ → Redeploy** → esperar **Ready**.
   (Env var nova só vale depois do redeploy.)
5. Teste: `https://github-readme-stats-nine-ashy-18.vercel.app/api?username=Renan0204&show_icons=true&count_private=true&cb=1`

## O que NÃO ficou self-hosted

- **Sequência de contribuições** (`streak-stats.demolab.com`): instância pública, está estável.
- **Gráfico de atividade** (`github-readme-activity-graph`): tentamos hospedar mas a
  instância própria não autenticava na API do GitHub (`Can't fetch any contribution`).
  Card **removido** do README. A "cobrinha" (`.github/workflows/snake.yml`) já cobre a
  parte visual de contribuições.
- **Troféus** (`github-profile-trophy`): projeto em Deno, não sobe na Vercel pelo runtime
  `vercel-deno` (erro de import no carregamento). Card **removido** do README.

## Observações

- **Cache da Vercel:** o `github-readme-stats` responde com `Cache-Control: max-age=86400`.
  Depois de um redeploy, um erro antigo pode continuar aparecendo por até 24h — teste
  sempre com um parâmetro extra na URL (`&cb=1`) para forçar resposta nova.
- **`count_private=true`:** só conta repositório privado se o token tiver escopo `repo`.
- **Plano Hobby da Vercel:** o projeto consome quase nada; não chega perto do limite.
