# ViabiCalc — Guia completo de deploy

Aplicativo de viabilidade econômica para projetos imobiliários.
Stack: React + Vite + Supabase (banco + autenticação) + Vercel (hospedagem).

---

## Pré-requisitos

- Node.js 20 ou superior → https://nodejs.org
- Conta gratuita no Supabase → https://supabase.com
- Conta gratuita na Vercel → https://vercel.com
- Git instalado → https://git-scm.com

---

## Parte 1 — Configurar o Supabase (banco de dados + login)

### 1.1 Criar o projeto

1. Acesse https://supabase.com e clique em **New project**
2. Escolha um nome (ex.: `viabiCalc`) e uma senha forte para o banco
3. Selecione a região **South America (São Paulo)**
4. Aguarde ~2 minutos enquanto o projeto é criado

### 1.2 Criar as tabelas

1. No painel do Supabase, clique em **SQL Editor** (menu lateral)
2. Clique em **New query**
3. Cole todo o conteúdo do arquivo `supabase/schema.sql`
4. Clique em **Run** (ou Ctrl+Enter)
5. Você verá a mensagem "Success. No rows returned" — está correto

### 1.3 Habilitar login com Google (opcional, mas recomendado)

1. No Supabase, vá em **Authentication → Providers → Google**
2. Ative o toggle **Enable Google provider**
3. Você precisará de um Client ID e Client Secret do Google:
   - Acesse https://console.cloud.google.com
   - Crie um projeto → APIs & Services → Credentials → OAuth 2.0
   - Em "Authorized redirect URIs", adicione:
     `https://XXXX.supabase.co/auth/v1/callback`
     (substitua XXXX pelo ID do seu projeto Supabase)
4. Cole o Client ID e Secret no Supabase e salve

### 1.4 Copiar as chaves de API

1. No Supabase, vá em **Settings → API**
2. Copie:
   - **Project URL** → `https://XXXX.supabase.co`
   - **anon / public key** → começa com `eyJ...`
3. Guarde esses valores — você vai precisar na próxima etapa

---

## Parte 2 — Configurar o projeto localmente

### 2.1 Clonar e instalar

```bash
# Clone o repositório (ou crie uma pasta nova e copie os arquivos)
git clone https://github.com/seu-usuario/viabiCalc.git
cd viabiCalc

# Instale as dependências
npm install
```

### 2.2 Configurar variáveis de ambiente

```bash
# Copie o arquivo de exemplo
cp .env.example .env
```

Abra o arquivo `.env` no seu editor e preencha:

```
VITE_SUPABASE_URL=https://XXXXXXXXXXXX.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### 2.3 Rodar localmente

```bash
npm run dev
```

Acesse http://localhost:5173 no navegador.
Crie uma conta com e-mail e senha ou use o Google.

---

## Parte 3 — Deploy na Vercel (gratuito, online em 5 minutos)

### 3.1 Subir o código no GitHub

```bash
# Se ainda não tem repositório:
git init
git add .
git commit -m "ViabiCalc v1.0"

# Crie um repositório em github.com, depois:
git remote add origin https://github.com/seu-usuario/viabiCalc.git
git push -u origin main
```

### 3.2 Conectar à Vercel

1. Acesse https://vercel.com e faça login com o GitHub
2. Clique em **Add New → Project**
3. Selecione o repositório `viabiCalc`
4. A Vercel detecta automaticamente que é um projeto Vite/React

### 3.3 Configurar variáveis de ambiente na Vercel

Antes de clicar em Deploy, expanda **Environment Variables** e adicione:

| Nome | Valor |
|------|-------|
| `VITE_SUPABASE_URL` | `https://XXXX.supabase.co` |
| `VITE_SUPABASE_ANON_KEY` | `eyJ...` |

### 3.4 Deploy

Clique em **Deploy**. Em ~2 minutos seu app estará em:
`https://viabiCalc.vercel.app` (ou URL gerada automaticamente)

### 3.5 Atualizar o Supabase com a URL de produção

1. No Supabase → **Authentication → URL Configuration**
2. Em **Site URL**, coloque: `https://viabiCalc.vercel.app`
3. Em **Redirect URLs**, adicione: `https://viabiCalc.vercel.app/**`

---

## Parte 4 — Domínio personalizado (opcional)

### Na Vercel:
1. Vá em **Settings → Domains**
2. Adicione seu domínio (ex.: `viabiCalc.com.br`)
3. Siga as instruções para configurar o DNS

### No registro.br (domínios .com.br):
- Custo: ~R$ 40/ano
- Acesse https://registro.br e configure os nameservers para a Vercel

---

## Estrutura de arquivos gerados

```
viabiCalc/
├── supabase/
│   └── schema.sql          ← Script SQL (rode no Supabase)
├── src/
│   ├── lib/
│   │   ├── supabase.js     ← Cliente Supabase
│   │   ├── AuthContext.jsx ← Contexto de autenticação
│   │   ├── api.js          ← Todas as chamadas ao banco
│   │   └── calc.js         ← Motor de cálculo (VPL, TIR, etc.)
│   ├── pages/
│   │   ├── Login.jsx       ← Tela de login
│   │   └── Dashboard.jsx   ← Lista de projetos
│   └── App.jsx             ← Roteamento principal
├── .env.example            ← Modelo das variáveis de ambiente
├── package.json
└── README.md
```

---

## Dúvidas frequentes

**P: Preciso de cartão de crédito?**
R: Não. Supabase e Vercel têm planos gratuitos generosos para MVPs.

**P: O Supabase gratuito tem limite?**
R: Sim — 500 MB de banco, 50.000 usuários ativos/mês, 2 GB de transferência.
Para um MVP ou produto inicial, é mais do que suficiente.

**P: Como adicionar novas telas (projeto, resultados)?**
R: Crie arquivos em `src/pages/` e adicione as rotas no `App.jsx`.
A lógica de cálculo já está pronta em `src/lib/calc.js`.

**P: Como gerar PDF dos resultados?**
R: Instale `npm install jspdf html2canvas` e chame `html2canvas` na div
de resultados, depois `jsPDF.addImage()`. Posso gerar esse código se precisar.
