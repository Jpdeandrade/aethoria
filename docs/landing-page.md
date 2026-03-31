# Landing Page — Aethoria

Documentação técnica e de conteúdo da LP em `aethoria.online`.

---

## Arquivo

`aethoria-landing-page.html` — single file com HTML, CSS e JS embutidos.

---

## Fontes

| Font | Weights | Uso |
|---|---|---|
| Cinzel | 400, 600, 700, 900 | Headings, labels, botões, nav |
| Crimson Text | 400, 400i, 600 | Body, subtítulos, tooltips |

---

## Variáveis CSS principais

```css
--color-bg-dark:       #0a0a0f
--color-bg-deep:       #050508
--color-primary:       #d4af37   /* ouro */
--color-primary-light: #f0d98f
--color-accent:        #4a90e2   /* azul tech */
--color-text:          #e8e8f0
--color-text-dim:      #a8a8b8
--color-border:        rgba(212, 175, 55, 0.2)

--clip-card: polygon(12px 0%, ...)   /* octagonal */
--clip-btn:  polygon(8px 0%, ...)    /* paralelogramo */

--h-pad: clamp(7%, calc(50% - 490px), 20%)  /* largura máx ~980px */
```

---

## Estrutura de seções

| # | Seção | ID / Classe | Descrição |
|---|---|---|---|
| 1 | Hero | `.hero` | Headline, subtítulo, 2 CTAs |
| 2 | O Que É | `.central-concept` | Metáfora + grid É/Não É |
| 3 | Animação | `.universe-demo` | SVG interativo — 3 campanhas |
| 4 | O Conceito | `#conceito` | 6 cards de diferenciais |
| 5 | Para Quem | `#para-quem` | 3 personas com dores e benefícios |
| 6 | Como Funciona | `#como-funciona` | Timeline de 5 passos |
| 7 | Build in Public | `.cta-section` | Contexto do projeto + links |
| 8 | Cadastro | `#cadastro` | Formulário Nome / E-mail / WhatsApp |

---

## Animação SVG — "Três Mesas, Uma História"

### Elementos

| ID | Elemento | Descrição |
|---|---|---|
| `#universe-svg` | SVG desktop | viewBox 0 0 980 300 |
| `#universe-svg-v` | SVG mobile | viewBox 0 0 320 570 |
| `#path-dnd` | Path D&D | `M 145 70 L 670 70 C 718 70 718 150 718 150` |
| `#path-coc` | Path CoC | `M 145 150 L 718 150` |
| `#path-pf` | Path PF | `M 145 230 L 670 230 C 718 230 718 150 718 150` |
| `#path-canon` | Path Canon | `M 718 150 L 958 150` |
| `#conv-node` | Nó de convergência | `cx=718 cy=150` |

### Durações

| Etapa | Duração |
|---|---|
| 3 lanes simultâneas | 3500ms |
| Pausa antes do canon | 380ms |
| Canon | 2200ms |
| Fade-in de labels | 500ms (CSS transition) |

### Cores

```
D&D 5e:      #d4af37 (ouro)
CoC 7e:      #4a90e2 (azul)
Pathfinder:  #9b6fa5 (roxo)
Canon:       #f0d98f (ouro claro)
```

### Lógica JS

```javascript
// Detecta SVG ativo (desktop ou mobile)
getConfig() → DESKTOP | MOBILE

// Viagem do orb
travel(pathId, dotSel, color, duration, onDone, axis)
  → getPointAtLength() + requestAnimationFrame
  → pré-computa progress de cada dot em 120 steps
  → ativa dot + event-group quando orb passa

// Burst de convergência
burst(cx, cy) → 4 anéis expandindo (dnd, coc, pf, white)

// Scroll trigger
IntersectionObserver threshold: 0.25
```

### Comportamento mobile

- SVG horizontal (`#universe-svg`) oculto via CSS `display: none`
- SVG vertical (`#universe-svg-v`) ativo via `display: block`
- Tooltip flutuante substituído por painel fixo abaixo do SVG
- Hint "✦ Toque nos eventos para saber mais" desaparece na primeira interação
- Axis de trigger muda de `x` para `y`

---

## Formulário de Cadastro

### Validação

- Nome: obrigatório, não vazio
- E-mail: obrigatório + regex `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`
- WhatsApp: opcional

### Submit

```javascript
fetch(WEBHOOK_URL, {
    method: 'POST',
    mode: 'no-cors',                                  // evita preflight CORS
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({ name, email, whatsapp })
}).finally(() => showSuccess())
```

**Nota:** `mode: 'no-cors'` impede leitura da resposta — sempre exibe sucesso após envio. Os dados chegam normalmente no webhook.

---

## Responsividade

| Breakpoint | Mudanças principais |
|---|---|
| ≤ 768px | Nav simplifica (só nav-cta visível), grids para 1 coluna |
| ≤ 480px | Font-size 16px, SVG horizontal → vertical, logo-mark oculto |

---

## Links

| Destino | URL |
|---|---|
| LinkedIn | https://www.linkedin.com/in/joaopedrodeandrade/ |
| GitHub | https://github.com/Jpdeandrade/aethoria |
| Webhook | https://webhook.infrajp.xyz/webhook/cba6d127-f8b1-44c3-89a8-b42b8340d91d |
