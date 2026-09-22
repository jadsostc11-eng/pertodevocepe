# Publicação no Cloudflare Workers

Este portal não é um site estático: os cadastros e avaliações usam um banco D1 e as fotos usam R2. Por isso, publique-o como **Cloudflare Worker**.

## 1. Preparar o computador

Instale o Node.js 22 ou superior. Na pasta do projeto, execute:

```bash
npm ci
npx wrangler login
```

## 2. Criar os recursos no Cloudflare

```bash
npx wrangler d1 create perto-de-voce-db
npx wrangler r2 bucket create perto-de-voce-fotos
```

Copie o `database_id` retornado pelo primeiro comando e substitua `SUBSTITUA_PELO_ID_DO_BANCO_D1` em `wrangler.jsonc`.

## 3. Criar as tabelas e os segredos

```bash
npx wrangler d1 migrations apply perto-de-voce-db --remote --migrations-dir=./drizzle
npx wrangler secret put ADMIN_PASSWORD
npx wrangler secret put ADMIN_SESSION_SECRET
```

Use uma senha forte para a área administrativa. Para `ADMIN_SESSION_SECRET`, use uma sequência aleatória longa, diferente da senha.

## 4. Publicar

```bash
npm run deploy
```

Após a publicação, abra o Worker no painel Cloudflare e conecte o domínio `www.pertodevocepe.com.br` em **Workers & Pages > perto-de-voce-pe > Settings > Domains & Routes**. Aponte o DNS do domínio para o Worker conforme o painel orientar.

## Atualizações futuras

Depois de alterar o código, basta executar novamente:

```bash
npm run deploy
```

Para testar antes de publicar, use `npm run dev`.
