# JobDay — Araçatuba

Marketplace regional de bicos (presenciais e remotos), conectado ao Supabase.
Front-end estático (`index.html`, sem build step) + banco Postgres/Supabase com
RLS, triggers de piso mínimo, regra dos 3 erros e avaliação bilateral cega.

## Estrutura

```
index.html     -> app inteiro (front-end estático, chama a API REST do Supabase direto)
vercel.json    -> config mínima de deploy estático
```

## Rodando localmente

Não precisa de build. Só abrir o `index.html` no navegador, ou servir com:

```bash
npx serve .
```

> Importante: se você tentar abrir o `index.html` dentro do preview de arquivo do
> Claude, o `fetch()` para o Supabase será bloqueado pelo sandbox do navegador
> (erro "Failed to fetch"). Isso não acontece rodando localmente ou já publicado
> na Vercel — só é uma limitação da pré-visualização em chat.

## Deploy na Vercel

1. Suba este diretório para um repositório no GitHub (veja passo a passo abaixo)
2. Em vercel.com → **Add New Project** → importe o repositório
3. Framework preset: **Other** (é HTML estático, sem build command, sem output directory)
4. Deploy

## Subindo para o GitHub

```bash
cd jobday-aracatuba
git init
git add .
git commit -m "Primeira versão do JobDay Araçatuba"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git
git push -u origin main
```

## Sobre a chave do Supabase no código

A `anon key` está hardcoded no `index.html` de propósito — ela é uma chave
**pública** por design no Supabase (o mesmo tipo de chave que qualquer app
front-end usa). Quem realmente protege os dados é o **RLS** configurado no
banco (cada tabela só libera o que cada usuário pode ver/editar). Não é
necessário nem recomendado tentar "esconder" essa chave em variável de
ambiente para este tipo de projeto.

## Banco de dados

Projeto Supabase: `portifólio` (região `sa-east-1`).
Schema completo (tabelas, triggers, RLS) já aplicado via migrations — veja o
histórico de migrations no painel do Supabase (Database → Migrations) para o
detalhamento de cada etapa (`gig_platform_01` a `gig_platform_06`).
