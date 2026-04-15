# Agenda Mensal — Igreja da Cidade

Visualizador de agenda mensal integrado ao sistema **Qhoras**, com painel de administração para edição e controle de visibilidade dos eventos.

🔗 **Site:** https://camillojoao1-afk.github.io/agenda-mensal-igreja

---

## Arquivos do projeto

| Arquivo | Descrição |
|---|---|
| `index.html` | Calendário público — página principal |
| `admin.html` | Painel de administração (protegido por senha) |
| `worker.js` | Proxy CORS para o Cloudflare Workers |

---

## Funcionalidades

### Calendário público (`index.html`)
- Visualização em **calendário**, **por dia** e **por sala**
- Atualização automática a cada 5 minutos
- Filtro por espaço/sala
- Estatísticas do mês (total de eventos, espaços, dia mais cheio)
- Layout responsivo — mobile e desktop
- Tema escuro

### Painel admin (`admin.html`)
- Acesso protegido por senha
- Edição de **nome**, **horário** e **sala** de cada evento
- **Ocultar/mostrar** eventos individualmente
- Salvar alterações por linha ou todas de uma vez
- Restaurar eventos ao dado original do Qhoras
- Filtrar por visibilidade e por sala
- Exportar e importar edições em JSON (para sincronizar entre dispositivos)

---

## Setup do Proxy CORS (obrigatório para funcionar online)

A API do Qhoras bloqueia chamadas vindas de navegadores em outros domínios (CORS).
A solução é um **Cloudflare Worker** gratuito que atua como intermediário servidor-a-servidor.

### Passo 1 — Criar conta no Cloudflare
Acesse [workers.cloudflare.com](https://workers.cloudflare.com) e crie uma conta gratuita (sem cartão de crédito).

### Passo 2 — Criar o Worker
1. No painel, clique em **Workers & Pages → Create**
2. Clique em **Create Worker**
3. Dê o nome `qhoras-proxy` e clique em **Deploy**
4. Clique em **Edit code**
5. Apague todo o código existente e cole o conteúdo do arquivo `worker.js`
6. Clique em **Deploy**
7. Copie a URL gerada — será algo como:
   ```
   https://qhoras-proxy.SEU-USUARIO.workers.dev
   ```

### Passo 3 — Configurar os arquivos HTML
Abra o `index.html` **e** o `admin.html`. Em cada um, localize esta linha perto do topo do `<script>`:

```js
const PROXY_URL = 'https://SEU-WORKER.workers.dev';
```

Substitua pela URL real do seu Worker:

```js
const PROXY_URL = 'https://qhoras-proxy.SEU-USUARIO.workers.dev';
```

Salve ambos os arquivos, faça o commit e o site estará funcionando. ✅

---

## Como alterar a senha do admin

A senha padrão de primeiro acesso é definida internamente no código. Para alterá-la de forma segura:

1. Acesse `admin.html` e faça login com a senha atual
2. Abra o console do navegador (`F12 → Console`)
3. Execute o comando abaixo com a sua nova senha:
   ```js
   localStorage.setItem('adminPwd', 'SuaNovaSenha')
   ```
4. Recarregue a página — a nova senha já estará ativa

> A senha fica armazenada apenas no navegador onde foi definida. Em um novo dispositivo, use a senha padrão e redefina novamente.

---

## Como usar o Export/Import de edições

As edições feitas no painel admin ficam salvas no `localStorage` do navegador.
Para levar as edições para outro dispositivo:

1. No painel admin, clique em **⬇ Exportar JSON**
2. Um arquivo `.json` será baixado com todas as edições
3. No outro dispositivo, acesse o admin e clique em **⬆ Importar JSON**
4. Selecione o arquivo — as edições serão aplicadas imediatamente

---

## Segurança

- Repositório **privado** — acesso restrito ao proprietário
- Nenhuma senha, token ou chave de API exposta no código
- O `BID` é o identificador público do negócio, visível na URL pública do Qhoras
- O Cloudflare Worker pode ter o `ALLOWED_ORIGIN` restrito ao seu domínio para maior proteção
- A senha do admin é armazenada apenas no `localStorage` do navegador — nunca enviada a servidores

---

## Tecnologias

- HTML5 + CSS3 + JavaScript puro (zero dependências de build)
- Fontes: Google Fonts — Playfair Display + DM Sans + DM Mono
- API: `reservation.qhoras.com/calendarPublic`
- Proxy: Cloudflare Workers (plano gratuito)
- Hospedagem: GitHub Pages
