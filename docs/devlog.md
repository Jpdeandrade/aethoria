# Devlog — Aethoria: Living Universe

Registro cronológico de tudo que foi construído, decidido e aprendido no desenvolvimento do Aethoria.
Parte do compromisso de **Build in Public**.

---

## Fase 1 — Infraestrutura e Deploy (Mar 2026)

### VPS e Stack de Hospedagem

**Decisão:** Hospedar em VPS própria em vez de Vercel/Netlify para ter controle total e custo previsível.

**Stack escolhida:**
- VPS: 46.224.29.188
- Orquestração: Docker Swarm
- Reverse proxy / SSL: Traefik v3.5.3
- Painel de containers: Portainer
- Deploy de sites estáticos: Nginx (via container por site)

**Estrutura de pastas na VPS:**
```
/root/sites/
  aethoria.online/
    html/          ← arquivos do site
    docker-compose.yml
```

**Processo de deploy de novo site:**
1. Script `novo-site.sh` cria a estrutura e o `docker-compose.yml`
2. `docker stack deploy -c docker-compose.yml aethoria`
3. Traefik detecta automaticamente e provisiona SSL via Let's Encrypt

**Problema encontrado:** O script `novo-site.sh` gerou labels do Traefik com escape incorreto (`Host(\)` vazio). Solução: sobrescrever o `docker-compose.yml` manualmente via heredoc SSH com backticks corretos.

**Atualização de conteúdo (workflow atual):**
```bash
scp -i ~/.ssh/notebook arquivo.html root@46.224.29.188:/root/sites/aethoria.online/html/index.html
ssh -i ~/.ssh/notebook root@46.224.29.188 "docker service update --force aethoria_aethoria"
```

---

## Fase 2 — Landing Page (Mar 2026)

### Decisão: Single-file HTML

**Por quê:** MVP de LP não justifica framework. Um único arquivo HTML/CSS/JS é suficiente, fácil de versionar e de atualizar na VPS com um SCP.

### Tema Visual: Medieval-Tech

**Decisão:** Identidade visual que une fantasia medieval (RPG) com tecnologia (produto digital).

| Elemento | Escolha | Motivo |
|---|---|---|
| Font display | Cinzel (serif) | Medieval, elegante, legível |
| Font body | Crimson Text | Editorial, fantasia literária |
| Cor primária | `#d4af37` (ouro) | Medieval, prestígio |
| Cor acento | `#4a90e2` (azul) | Tech, digital |
| Background | `#0a0a0f` | Dark mode, imersivo |
| Botões | Clip-path paralelogramo | Angularidade tech |
| Cards | Clip-path octagonal | Cuts medievais |

**Background:** Grid dourado sutil (50px) + gradientes radiais — "pergaminho digital".

### Layout

**Decisão:** CSS variable `--h-pad: clamp(7%, calc(50% - 490px), 20%)` aplicada globalmente.
- Conteúdo máximo: ~980px no desktop
- Mobile: 7% de padding lateral
- Única variável controla largura de todas as seções e header

### Estrutura de Seções (ordem final)

1. **Hero** — Headline EN, subtítulo PT, 2 CTAs
2. **O Que É Aethoria** — Metáfora + grid É/Não É
3. **Três Mesas, Uma História** — Animação SVG interativa
4. **O Conceito** — 6 cards com os diferenciais (I–VI)
5. **Para Quem É** — 3 personas: Jogadores, Comunidade, Mestres
6. **Como Funciona** — Timeline de 5 passos
7. **Build in Public** — Contexto do projeto, links
8. **Entre na Lista** — Formulário de cadastro

**Decisão de ordem:** "Build in Public" antes do formulário. Lógica: convencer → capturar. O contexto do projeto justifica o cadastro.

---

## Fase 3 — Animação Interativa (Mar 2026)

### "Três Mesas, Uma História"

**Conceito:** Ilustração interativa mostrando 3 campanhas simultâneas convergindo para lore oficial.

**Decisão técnica:** SVG puro + `requestAnimationFrame` (sem bibliotecas). Motivo: controle total, sem dependências, performance nativa.

**Implementação:**

```
SVG viewBox: 0 0 980 300
Lanes: D&D y=70 | CoC y=150 | PF y=230
Convergência: x=718 y=150
Canon: x=718→958 y=150
```

- Orbs viajam via `getPointAtLength()` ao longo dos paths SVG
- Dots são ativados quando o orb passa pelo `cx` correspondente (pré-computado em 120 steps)
- `IntersectionObserver` dispara a animação quando a seção entra na viewport
- Duração: 3500ms por lane, 2200ms para o canon

**Labels de evento:** Cada dot tem um `<g class="event-group">` com `<line>` (tick) + `<text>`. Aparecem com fade-in quando o orb passa.

**Versão mobile (≤480px):** SVG vertical separado (`viewBox 0 0 320 570`). Lanes lado a lado descendo de cima para baixo. JS detecta qual SVG está visível com `getComputedStyle` e usa o config correto (eixo Y em vez de X para trigger dos dots).

### A História da Animação

**Decisão de conteúdo:** Os eventos precisam ser simultâneos e interdependentes — não sequenciais. A história final:

> **"A Noite da Torre Sombria"** — Culto Sombrio realiza ritual de invocação.
> - **D&D 5e (Aventureiros):** Infiltram a Torre, banem a entidade
> - **CoC 7e (Investigadores):** Rastreiam desaparecimentos, mapeiam o culto, descobrem a data do ritual e repassam o intel
> - **PF 2e (Guarda):** Evacuam civis, cercam a Torre, interceptam fugitivos
> - **Lore Oficial:** Culto Sombrio extinto + Pacto das Três Chamas

O evento de CoC "Ritual Esta Noite" é o elo central — passa informação para D&D atacar e para PF cercar simultaneamente.

### Tooltip (Desktop) e Caption Panel (Mobile)

**Desktop:** Tooltip flutuante que segue o mouse, posicionado relativamente ao wrapper.

**Mobile:** Tooltip flutuante não funciona em touch (sai da tela). Solução: painel fixo abaixo do SVG que abre ao tocar no dot. Hint "✦ Toque nos eventos para saber mais" aparece antes da primeira interação.

---

## Fase 4 — Formulário e Integração (Mar 2026)

### Formulário de Cadastro

**Campos:** Nome (obrigatório), E-mail (obrigatório, validado por regex), WhatsApp (opcional).

**Validação:** Inline, com mensagens de erro em vermelho + borda vermelha no campo inválido.

**Submit:** Fade-out do form → estado de sucesso com "✦ Você está na lista."

**Integração:** Webhook próprio via n8n.

**Problema encontrado:** CORS bloqueou `fetch` com `Content-Type: application/json` (preflight OPTIONS rejeitado pelo servidor).

**Solução:** `mode: 'no-cors'` + `Content-Type: application/x-www-form-urlencoded` — evita o preflight. Desvantagem: sem acesso à resposta, então sempre mostra sucesso. Dados chegam normalmente no webhook.

```javascript
fetch('https://webhook.infrajp.xyz/webhook/...', {
    method: 'POST',
    mode: 'no-cors',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({ name, email, whatsapp })
})
```

---

## Fase 5 — Revisão UX (Mar 2026)

### Análise e melhorias aplicadas

| Problema | Solução |
|---|---|
| CTA hero ia para LinkedIn | Agora aponta para `#cadastro` |
| Header sem navegação | Nav adicionado com 4 âncoras |
| Seção "Valores" redundante com "Para Quem" | Removida |
| "Build in Public" depois do formulário | Reordenado: convence → captura |
| 6 bullets por persona | Reduzido para 4 (mais impacto) |
| Sem meta tags OG | Adicionados og:title, og:description, theme-color |
| Hint de hover longe do SVG | Movido para logo abaixo do SVG |
| Título formulário focado no criador | Mudado para "Entre na Lista" (foco no produto) |

### Meta tags adicionadas

```html
<meta name="description" content="...">
<meta property="og:title" content="Aethoria: Living Universe">
<meta property="og:description" content="...">
<meta property="og:url" content="https://aethoria.online">
<meta property="og:type" content="website">
<meta name="theme-color" content="#d4af37">
```

---

## Decisões de Produto Registradas

| Decisão | Escolha | Alternativa descartada | Motivo |
|---|---|---|---|
| Idioma da headline | Inglês | Português | Alcance global, posicionamento |
| Subtítulo hero | Português | Inglês | Público-alvo BR, contexto emocional |
| Foco da plataforma | Narrativa | Mecânicas | Diferencial único, sem concorrência direta |
| Personagem vinculado a sistema | Imutável | Flexível | Consistência narrativa, sem desvios |
| Curadoria | Staff humano | Automática | Qualidade > velocidade |
| Tempo no universo | Unificado ("presente") | Paralelo | Coerência narrativa entre mesas |
| Monetização no MVP | Nenhuma | Freemium | Crescimento orgânico primeiro |

---

## Stack de Desenvolvimento

| Ferramenta | Uso |
|---|---|
| Claude Code (Anthropic) | Desenvolvimento assistido por IA |
| VPS + Docker Swarm | Hospedagem |
| Traefik v3.5.3 | Reverse proxy + SSL |
| n8n | Automações + webhook do formulário |
| Git + GitHub | Versionamento |

---

## Próximos Passos

- [ ] PRD Técnico completo
- [ ] Iniciar desenvolvimento do MVP (Next.js + Supabase)
- [ ] Autenticação e perfil de jogador
- [ ] Quadro de missões
- [ ] Painel de curadoria (staff)
- [ ] Beta fechado com primeiros jogadores da lista
