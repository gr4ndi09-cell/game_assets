# Quiz sem API — Didactic Fighter

## O que mudou

A sequência agora é: **seleção de personagens, modo e mapa → escolha do tema → combate**. A tela de temas mantém o boss animado e a música do menu. O botão Voltar preserva as escolhas anteriores.

Foram removidas a chave de API, as chamadas ao Gemini, a geração por texto livre e a explicação por IA. As correções são locais. O som `quiz_generate.mp3`, já preparado na versão anterior, passa a tocar ao selecionar um tema, com fallback para `click.mp3`.

| Arquivo em assets/quizzes/ | Tema | Questões |
|---|---|---:|
| matematica.json | Matemática básica do ensino médio — padrão | 70 |
| historia_brasil.json | República Velha, Vargas, ditadura militar, redemocratização e Nova República | 70 |
| geopolitica.json | Conflitos, energia, recursos, logística e regiões estratégicas | 70 |
| portugues.json | Análise sintática e classificação tradicional escolar das orações | 70 |

São **280 questões**, cada uma com quatro alternativas, uma resposta correta e explicação curta. O jogo percorre as questões em ordem embaralhada antes de reutilizá-las e embaralha as alternativas a cada apresentação. O tema vale para todos os jogadores e para os quizzes de fruta e Última Chance.

## Instalação

1. Substitua o HTML anterior pelo novo `indexchat.html`.
2. Envie os quatro JSONs para `assets/quizzes/` no repositório de assets (`game_assets`). O ZIP já contém essa estrutura.
3. Se o HTML e os assets forem hospedados juntos, mantenha a pasta `assets/quizzes/` ao lado do HTML.
4. Recarregue a página e escolha um tema antes de iniciar a partida.

Ao selecionar um tema, o jogo fica imediatamente pronto com o banco completo incluído no HTML. Em paralelo, tenta carregar `assets/quizzes/<tema>.json` junto à página e, depois, o mesmo caminho no repositório de assets configurado em `REPO_ROOT`. Cada tentativa de rede tem limite de 1,8 segundo. Ao abrir por `file://`, pula a tentativa de leitura local via fetch.

Se encontrar um JSON válido antes do início da partida, usa esse conteúdo. Se não encontrar, mantém as 70 perguntas incluídas **do tema selecionado**. Iniciar o combate cancela a atualização pendente; nenhuma resposta atrasada troca o tema no meio da partida. O início não espera a rede.

**A garantia sem rede vale para o quiz.** Modelos, imagens, músicas, vídeos e bibliotecas externas do restante do jogo continuam com as referências anteriores. Este pacote não transforma o jogo inteiro em uma instalação offline.

## Como editar os JSONs

O formato de geração anterior foi preservado, com o campo adicional opcional `explanation`:

```json
{
  "questions": [
    {
      "q": "Quanto é 15% de 200?",
      "options": ["15", "30", "45", "20"],
      "correct": 1,
      "explanation": "15/100 × 200 = 30."
    }
  ]
}
```

`correct` começa em **zero**: 0 = primeira alternativa, 1 = segunda, 2 = terceira, 3 = quarta. Não use comentários nem vírgula após o último item. Cada questão precisa de quatro alternativas distintas. A validação rejeita o banco inteiro se uma questão estiver estruturalmente inválida, mantendo a cópia interna.

Os arquivos entregues têm 70 questões cada. O leitor aceita bancos personalizados de 1 a 1.000 questões; para o evento, mantenha a faixa desejada de 70 a 120. Alterar um JSON externo não altera automaticamente a cópia de segurança `BUILTIN_QUIZZES` no HTML. Para que suas edições também funcionem sem rede, atualize essa cópia com o mesmo conteúdo antes do evento.

Recarregue a página depois de atualizar o repositório. Se começar imediatamente, enquanto o banco externo ainda carrega, a partida usará a cópia incluída. O contador da tela mostra quantas perguntas estão prontas.

## Conteúdo e referências

Questões redigidas para este jogo, sem reprodução de bancos de provas. Matemática tem dificuldade básica de ensino médio, incluindo exercícios de fixação com valores diferentes. Português segue a nomenclatura tradicional usada na escola.

Geopolítica foi revisada em **14/10/2026** (horário de Brasília). O banco prioriza fundamentos e acontecimentos com datas explícitas; não depende de cotações, contagens de vítimas, líderes atuais ou de afirmar que uma guerra terminou ou está em determinada fase. Revise o JSON se desejar acrescentar notícias posteriores.

Referências de consulta para fatos históricos e geopolíticos:

- [TSE — Cronologia das eleições](https://www.tse.jus.br/eleicoes/historia/cronologia-das-eleicoes): datas e modalidades de eleição.
- [TSE — Eleição de 1985 e Colégio Eleitoral](https://www.tse.jus.br/jurisprudencia/julgados-historicos/eleicao-de-1985-fidelidade-partidaria-no-colegio-eleitoral): Tancredo, Sarney e eleição indireta.
- [TSE — Eleições anteriores](https://www.tse.jus.br/eleicoes/eleicoes-anteriores): eleições presidenciais posteriores.
- [Senado — Anais das Constituintes](https://www25.senado.leg.br/web/atividade/anais/anais-das-constituintes): história constitucional e Estado Novo.
- [TRE-RJ — Contexto histórico 1930–1945](https://www.tre-rj.jus.br/institucional/memoria/eventos-eleitorais/contexto-historico/1930-1945): período varguista.
- [Arquivo Nacional — AI-5](https://www.gov.br/memoriasreveladas/pt-br/centrais-de-conteudo/destaques/instituido-o-ai-5): repressão e restrição de direitos.
- [EIA — World Oil Transit Chokepoints](https://www.eia.gov/international/analysis/special-topics/world_oil_transit_Chokepoints): rotas de petróleo e gargalos marítimos.
- [IEA — Critical Minerals](https://www.iea.org/topics/critical-minerals): minerais e transição energética.
- [IEA — The Future of Hydrogen](https://www.iea.org/reports/the-future-of-hydrogen): produção e usos do hidrogênio.
- [IEA — Ammonia Technology Roadmap](https://iea.blob.core.windows.net/assets/6ee41bb9-8e81-4b64-8701-2acc064ff6e4/AmmoniaTechnologyRoadmap.pdf): amônia, energia e fertilizantes.
- [GEUS — Mineral resources in Greenland](https://eng.geus.dk/mineral-resources/mineral-resources-in-greenland-an-overview): potencial mineral da Groenlândia.
- [Dinamarca — Greenland](https://denmark.dk/people-and-culture/greenland): autogoverno e relação com o Reino da Dinamarca.
- [ONU — Iniciativa de Grãos do Mar Negro](https://www.un.org/en/black-sea-grain-initiative/achievements): exportações agrícolas e segurança alimentar.
- [ONU — Resolução 68/262](https://digitallibrary.un.org/record/767883?ln=en): integridade territorial da Ucrânia.
- [Conselho Europeu — 24 de fevereiro de 2022](https://www.consilium.europa.eu/en/press/press-releases/2022/02/24/european-council-conclusions-24-february-2022/): invasão russa em larga escala.
- [OTAN — Relações com a Finlândia](https://www.nato.int/cps/en/natohq/topics_49594.htm?selectedLocale=us): ingresso finlandês em 2023 e sueco em 2024.
- [CFR — Taiwan e semicondutores](https://www.cfr.org/articles/why-taiwan-important-united-states): papel de Taiwan nas cadeias de tecnologia; o quiz não utiliza percentuais variáveis de produção.

## Verificação realizada

Validados os quatro JSONs, a quantidade de questões, as alternativas distintas, os índices de resposta e a sintaxe JavaScript. Testes automatizados cobriram o banco offline por tema, JSON externo válido/inválido, respostas de rede atrasadas, sorteio sem repetição por ciclo, preservação dos gabaritos após embaralhamento, retorno à seleção e proteção contra início duplicado.

A tela foi integrada ao código existente, mas não houve teste visual em navegador nesta entrega. Antes do evento, confira o fluxo nos dispositivos que serão usados.
