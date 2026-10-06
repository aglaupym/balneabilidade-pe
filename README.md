# NE1 · Balneabilidade → CSV

Lê o informativo de balneabilidade da CPRH (PDF ou print da 1ª página) e gera o CSV usado no gráfico do After Effects.

- **PDF:** lido no próprio navegador (pdf.js), sem custo e sem servidor.
- **Print/imagem:** lido por IA através da função `api/ler-imagem.js` (precisa de `ANTHROPIC_API_KEY`).
- Sem build e sem dependências npm: HTML, CSS e JavaScript puros.

## Estrutura
```
public/            site estático (é o que a Vercel publica)
  index.html
  css/style.css
  js/praias.js     lista das 27 praias (código CPRH -> nome da comp)
  js/app.js        leitura, tabela, CSV
  assets/          fundo e logo NE1
  vendor/          pdf.js (local, sem CDN)
api/ler-imagem.js  função serverless (leitura de print)
dev-server.js      servidor local (npm start)
vercel.json  package.json  .env.example
```

## Testar localmente
Requer Node.js 18 ou mais novo (`node -v`).
1. `cd balneabilidade-ne1`
2. (Só para testar print) `cp .env.example .env` e preencha `ANTHROPIC_API_KEY=`
3. `npm start`
4. Abra http://localhost:3000

Sem a chave, tudo funciona exceto a leitura de print (aparece a mensagem para enviar o PDF).

## Publicar na Vercel
**Pelo GitHub:** suba a pasta, em vercel.com clique em *Add New > Project*, importe o repositório e *Deploy* (sem alterar nenhuma configuração).
**Pela CLI:** `npm i -g vercel`, depois `vercel` (teste) e `vercel --prod`.

Para a leitura de print: *Project > Settings > Environment Variables* → adicione `ANTHROPIC_API_KEY` (e, se quiser, `ANTHROPIC_MODEL`, `RATE_LIMIT_PER_HOUR`) e faça um novo deploy.
O site é público e sem login. A leitura de print usa a **sua** chave: há limite por IP (padrão 20/hora, melhor esforço) e defina também um limite de gasto no console da Anthropic.
