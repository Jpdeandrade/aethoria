# Build in Public — Aethoria

## Propósito

Aethoria é desenvolvido em público: cada decisão técnica, pivô de produto e lição aprendida é documentada no LinkedIn e versionada no GitHub.

O projeto serve dois objetivos simultâneos:
- Construir um produto real de RPG narrativo
- Documentar o processo de construção como portfólio e aprendizado

**A regra central:** desenvolvimento e posts andam em trilhos separados. O desenvolvimento tende a estar à frente — e isso é intencional. O buffer entre o que foi construído e o que foi postado dá fôlego para manter a cadência semanal de posts mesmo em semanas de pouco progresso técnico.

---

## Trilho 1 — Desenvolvimento

### Concluído
- [x] Infraestrutura: VPS + Docker Swarm + Traefik + SSL automático
- [x] Landing page live em aethoria.online (single-file HTML/CSS/JS)
- [x] Animação SVG interativa ("Três Mesas, Uma História")
- [x] Formulário de cadastro integrado via n8n webhook
- [x] Briefing de Produto v2.0 validado (visão, escopo, regras, roadmap)

### Roadmap MVP (Next.js + Supabase)
- [ ] Setup do projeto Next.js + Supabase
- [ ] Autenticação e perfil de jogador
- [ ] Criação de personagem (vinculado a sistema, imutável)
- [ ] Quadro de missões com filtro por sistema
- [ ] Candidatura a missão (jogador → mestre)
- [ ] Finalização de missão (mestre atualiza nível, concede itens)
- [ ] Registro de evento pós-missão (proposta à curadoria)
- [ ] Painel de curadoria (staff aprova, edita, rejeita)
- [ ] Feed de eventos canônicos (timeline pública)
- [ ] Aethoria Crowns: ganho automático por missão concluída
- [ ] Loja da Guilda (gastar AC em benefícios narrativos)

### Backlog de Features (Pós-MVP)
- [ ] Mapa interativo de Aethoria
- [ ] Sistema de revivificação via missão especial
- [ ] Notificações in-app
- [ ] Ranking de personagens mais impactantes
- [ ] Timeline visual da lore
- [ ] Perfis públicos expandidos
- [ ] Sistema de conquistas/badges
- [ ] Backlog público de features na LP (transparência + engajamento da comunidade)

---

## Trilho 2 — Posts

### Publicados

**#1 — "Você pode achar inútil fazer essas coisas..."** → [post-01.md](posts/post-01.md)
- **Cobriu:** Introdução ao projeto, problema da compatibilidade cross-sistema, solução (personagem vinculado a sistema), trade-offs, por que documentar publicamente
- **Teaser deixado:** Como eventos viram história canônica sem virar caos?
- **Link:** https://www.linkedin.com/posts/joaopedrodeandrade_voc%C3%AA-pode-achar-in%C3%BAtil-fazer-essas-coisas-share-7445225525507203072-KU7D

**#2 — "Sobre o B.O de como fazer 10 histórias diferentes funcionarem..."** → [post-02.md](posts/post-02.md)
- **Cobriu:** Arquitetura Git-like para a lore (main, feature branch, commit, PR, code review, merge), por que o modelo é interno e a UX fica simples
- **Teaser deixado:** Como orquestrar o desenvolvimento e escolhas técnicas
- **Link:** https://www.linkedin.com/posts/joaopedrodeandrade_rpg-aethoria-github-share-7448008450321625088-aflg

### Backlog de Posts
> Pontos do desenvolvimento que já têm insumo suficiente para virar post, mas ainda não foram publicados.

- [ ] **A arquitetura Git para narrativas** — como a lore funciona como um repositório: missões = branches, eventos = commits, curadoria = code review, canonização = merge. Por que essa analogia resolve o problema de consistência.
- [ ] **Infraestrutura do zero** — VPS, Docker Swarm, Traefik, SSL automático. Por que não usei Vercel/Netlify e o que aprendi no processo.
- [ ] **A LP que virou um projeto** — o que começou como página simples virou uma animação SVG interativa com lógica de mobile separada. Decisões de design e o que não teria feito hoje.
- [ ] **O sistema de economia** — por que a moeda in-game (ouro D&D, dólares CoC) nunca vai para a plataforma, e como os Aethoria Crowns resolvem o engajamento cross-sistema sem virar pay-to-win.
- [ ] **Eventos como moeda narrativa** — o que é um evento, como ele entra na lore, por que curadoria humana (e não automática) é o diferencial.
- [ ] **Primeiros passos no MVP** — escolha de stack (Next.js + Supabase), por que não reinventei a roda, e o que o setup inicial ensinou.

---

## Cadência

- **Posts:** semanais, às quartas (a definir)
- **Formato:** LinkedIn, português BR, tom pessoal e direto
- **Estrutura típica:** problema da semana → tentativas → solução → trade-offs → teaser do próximo
- **GitHub:** repositório público em github.com/Jpdeandrade/aethoria

---

## Notas de Produto

- A LP pode receber uma seção de **backlog público**: features planejadas, algumas selecionadas para exibição. Objetivo: transparência no processo e potencial de engajamento da comunidade antes do lançamento.
