---
data: 2026-04-09
hashtags: #rpg #aethoria #github #git #produtodigital
link: https://www.linkedin.com/posts/joaopedrodeandrade_rpg-aethoria-github-share-7448008450321625088-aflg
---

Sobre o B.O de como fazer 10 histórias diferentes funcionarem dentro de Aethoria.

Pra quem não faz ideia de como funciona um RPG de mesa:

Jogar RPG é como escrever um livro. Cada sessão conta um capítulo de uma história. Normalmente, temos um autor escrevendo, então é tranquilo pois está tudo na cabeça dele. Agora imagina ter 10 autores escrevendo sobre o mesmo capitulo, ao mesmo tempo. Como garantir que tudo faça sentido?

Vem que te conto como resolvi essa parte do desenvolvimento.

Imagina 10 mesas (capítulos de uma história) rodando ao mesmo tempo no mesmo mundo.
- Mesa A mata o dragão.
- Mesa B ainda não sabe que o dragão foi morto e vai lá matar ele também.
- Mesa C tá negociando o tesouro do reino com o dragão.

Minha primeira ideia foi hierarquizar os eventos: Eventos globais travam ações locais até a Staff validar os acontecimentos, e depois publicar.

Mas como rastrear quem propôs o quê? Como resolver conflito entre duas mesas que tocaram na mesma região? Quanto mais eu pensava em como gerenciar, mais gambiarra aparecia.

Dai surgiu a clareza: estou estudando sobre o GitHub, tirando algumas certificações... Enfim, lendo sobre a estrutura de GIT, commits, branches, merges, pull requests.

Veio a clareza mental total: "Isso é exatamente o que eu preciso pra Aethoria."

O paralelo é quase perfeito:

- Lore canônica = main (estado oficial do mundo)
- Missão ativa = feature branch (trabalho isolado, não afeta nada até finalizar)
- Evento registrado = commit (mudança documentada, com autor e data)
- Evento pendente = pull request (aguardando revisão da staff)
- Curadoria = code review (coerência, qualidade, conflitos)
- Evento aprovado = merge (entra na lore pra sempre)

Mesa A finaliza a campanha. Propõe: "Dragão Vermelho derrotado na Torre dos 4 Picos." Staff revisa. Conflita com algo? Faz sentido na lore? Aprova. Mundo atualizado. Todo mundo tem acesso ao novo "capitulo" da história.

Detalhe Importante: esse modelo é interno. O jogador nunca vê "branch". O mestre nunca vê "commit". Eles veem missões, eventos, publicação na lore. A complexidade fica escondida e a experiência do usuário fica simples.

Às vezes a solução já existe. Você só precisa olhar pro problema certo.

Próximo passo: Estruturar como vou orquestrar o desenvolvimento e escolhas mais técnicas.

Quer acompanhar sobre o desenvolvimento de Aethoria? Vou deixar o link nos comentários.

#rpg #aethoria #github #git #produtodigital
