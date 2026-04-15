# Agenda Mensal — Igreja

Visualizador de agenda mensal integrado ao sistema **Qhoras**, desenvolvido para exibir reservas e eventos dos espaços da igreja em tempo real.

## Funcionalidades

- 📅 Visualização em **calendário**, **por dia** e **por sala**
- 🔄 Atualização automática a cada 5 minutos
- 🔍 Filtro por espaço/sala
- 📊 Estatísticas do mês (total de eventos, espaços usados, dia mais cheio)
- 📱 Layout responsivo (mobile e desktop)
- 🌙 Tema escuro elegante

## Como usar

Abra o arquivo `index.html` em qualquer servidor web:

### Opção 1 — Servidor local (Python)
```bash
python3 -m http.server 8080
# Acesse: http://localhost:8080
```

### Opção 2 — VS Code Live Server
Instale a extensão **Live Server** e clique em "Go Live".

### Opção 3 — GitHub Pages
Ative o GitHub Pages nas configurações do repositório (Settings → Pages → Branch: main).

> ⚠️ **Importante:** Abrir o arquivo diretamente como `file://` pode falhar por restrições de CORS do navegador. Use sempre um servidor web.

## Fonte dos dados

Os dados são carregados em tempo real da API pública do [Qhoras](https://reservar.qhoras.com).  
O ID do negócio (`BID`) presente no código é o mesmo que aparece na URL pública do calendário, portanto não é informação sensível.

## Segurança

- Nenhuma chave de API, senha ou token está armazenada no código
- Todos os dados exibidos são públicos (mesmos dados visíveis no calendário público do Qhoras)
- O repositório é **privado** — acesso restrito ao proprietário

## Tecnologias

- HTML5 + CSS3 + JavaScript puro (sem dependências externas de build)
- Fontes: Google Fonts (Playfair Display + DM Sans)
- API: Qhoras `reservation.qhoras.com/calendarPublic`
