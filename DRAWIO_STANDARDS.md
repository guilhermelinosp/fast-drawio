# Padrão obrigatório de diagramas do Fast

Este documento define as regras obrigatórias para criar, editar e revisar diagramas do Fast. Ele se aplica a toda alteração em `fast.drawio` e deve ser consultado antes de abrir um pull request ou fazer um commit.

## 1. Arquivo e páginas

- O repositório tem **um único arquivo de fonte**: `fast.drawio`, na raiz.
- Não crie arquivos `.drawio` paralelos, cópias de trabalho ou exportações versionadas.
- Cada página deve representar um recorte claro da arquitetura. Não duplique a mesma visão em páginas diferentes.
- Nomeie páginas com `C4 - <nível> - <escopo>` ou `Deploy - <escopo>`, em português, usando `-` como separador. Exemplos: `C4 - Contexto - Fast`, `C4 - Container - Plataforma`, `C4 - Componente - API`, `Deploy - Produção`.
- O nome deve ser estável, curto e único; renomear uma página exige registrar o motivo na descrição do commit.

## 2. Níveis C4

Use somente o nível adequado ao objetivo da página e não misture abstrações:

- **Contexto**: sistema Fast, usuários, organizações e sistemas externos; não mostre serviços internos.
- **Container**: aplicações, APIs, workers, bancos e filas que compõem o Fast; não mostre classes ou funções.
- **Componente**: módulos ou componentes dentro de um container; não expanda para infraestrutura física.
- **Deploy**: nós, ambientes, regiões, clusters e instâncias onde os containers são executados; não use este nível para explicar regras de negócio.

Uma página não pode misturar níveis. Quando for necessário detalhar um elemento, crie ou atualize a página do nível correspondente e faça referência a ela no label, sem colocar os dois níveis no mesmo desenho.

## 3. Paleta semântica

As cores comunicam significado, não decoração. Use cores claras, consistentes e legíveis em tema claro e escuro:

| Uso | Cor sugerida |
| --- | --- |
| Sistema Fast / elemento principal | azul `#DAE8FC` |
| Serviço, aplicação ou componente interno | verde `#D5E8D4` |
| Dados persistentes | amarelo `#FFF2CC` |
| Mensageria e eventos | laranja `#FFE6CC` |
| Usuário, ator ou sistema externo | cinza `#F5F5F5` |
| Limite de domínio ou grupo | roxo claro `#E1D5E7` |
| Alerta, risco ou estado inválido | vermelho claro `#F8CECC` |

Não use cor para representar dois conceitos diferentes na mesma página. Evite gradientes, sombras fortes, cores saturadas e texto com baixo contraste. Se uma exceção for indispensável, explique-a na legenda.

## 4. Formas por tipo

- Sistema, container, componente e serviço: retângulo ou retângulo arredondado.
- Usuário, ator ou pessoa: forma de pessoa/ator ou círculo claramente identificado.
- Banco, cache ou armazenamento: cilindro.
- Fila, tópico ou barramento: cilindro/forma de mensageria, sempre com label explícito.
- Decisão ou condição: losango; use apenas em fluxos, não para representar um serviço.
- Limite de domínio, sistema ou ambiente: contêiner/swimlane com título.
- Nota explicativa: documento ou anotação, visualmente distinta dos elementos arquiteturais.

Não use uma forma apenas por estética. A forma deve permitir inferir o tipo do elemento sem depender exclusivamente da cor.

## 5. Conectores, setas e eventos

- Use conectores ortogonais, com origem e destino claros; evite linhas que cruzem elementos.
- **Regra explícita de legibilidade: conectores nunca podem atravessar, cobrir ou encostar em textos**, sejam labels de nós, labels de arestas, títulos, legendas ou notas.
- Reserve espaço livre entre nós e ao redor de textos para que os conectores permaneçam visualmente separados; quando necessário, reposicione os nós ou use uma nota dedicada.
- Evite labels em arestas; prefira expressar a relação em texto dentro dos nós ou em notas explicativas. Use label na aresta somente quando for indispensável para entender o contrato e mantenha-o curto.
- Prefira conectores ortogonais com cantos arredondados e, na revisão via draw.io MCP, use `routing: "libavoid"` para rotear os conectores ao redor das formas e dos textos.
- Toda relação direcional usa seta na ponta do destino. Relações sem direção usam linha sem seta somente quando isso for realmente necessário.
- Labels de conectores devem ser verbos ou contratos curtos, por exemplo `consulta`, `publica`, `autentica` ou `lê`.
- Uma chamada síncrona deve usar linha contínua.
- Um evento ou comunicação assíncrona deve usar linha **tracejada**, seta no destino e label com o evento publicado, por exemplo `PedidoCriado`.
- Não use tracejado para representar chamadas síncronas, dependência genérica ou elementos opcionais sem registrar esse significado na legenda.
- Não ligue elementos sem uma relação arquitetural relevante. Reduza cruzamentos agrupando um fluxo em um broker, gateway ou log de eventos quando apropriado.

## 6. Limites de domínio

- Delimite bounded contexts, domínios, sistemas externos e ambientes com contêineres nomeados.
- Um elemento deve pertencer a um único limite de domínio na visão apresentada.
- Relações entre domínios devem atravessar o limite de forma visível e ter label do contrato ou integração.
- Não coloque usuários ou sistemas externos dentro do limite do Fast.
- Não use um limite de domínio para esconder elementos desconectos ou compensar um layout ruim.

## 7. Nomenclatura, IDs e labels

- Labels visíveis devem estar em português, salvo nomes oficiais de produtos, protocolos, APIs, eventos, tecnologias ou identificadores de código.
- Use nomes específicos e consistentes: `API de Pedidos`, `Banco de Pedidos`, `Fila de Notificações`; evite `Coisa`, `Serviço 1` e siglas sem definição.
- Padronize siglas e escreva a forma completa na primeira ocorrência ou na legenda.
- IDs internos do draw.io devem ser únicos no arquivo, estáveis durante uma edição e sem espaços, acentos ou caracteres especiais; prefira `tipo-escopo-nome` em `kebab-case`.
- Não renomeie IDs sem necessidade: eles ajudam a manter referências, diffs e revisão compreensíveis.
- Labels devem caber na forma, ter no máximo duas ou três linhas e não conter detalhes que pertencem a outro nível C4.
- Use HTML apenas quando necessário para hierarquia visual; escape caracteres XML corretamente.

## 8. Layout e grid

- Organize a leitura da esquerda para a direita ou de cima para baixo, sem mudar a direção no meio da página.
- Alinhe elementos em grid regular; use espaçamento uniforme e mantenha áreas livres para conectores.
- Coloque o elemento de entrada à esquerda/acima e o resultado à direita/abaixo. Agrupe elementos relacionados por domínio ou responsabilidade.
- Evite sobreposição, linhas diagonais longas, conectores passando por formas e páginas excessivamente largas.
- Mantenha margem externa, título da página e escala visual coerentes entre as páginas.
- Para diagramas gerados ou revisados por automação, preserve posições estáveis para que o diff XML seja revisável.

## 9. Legenda obrigatória

Toda página que usar mais de uma cor, forma ou tipo de conector deve conter uma legenda pequena, no canto inferior ou em área livre. A legenda deve explicar, quando aplicável:

- cores semânticas;
- formas dos tipos de elemento;
- linha contínua versus tracejada;
- significado das setas e siglas.

Não coloque na legenda regras já evidentes nem transforme a legenda em documentação extensa.

## 10. Versionamento e revisão

- Faça alterações somente em `fast.drawio` e nos documentos relacionados; não adicione exportações PNG, SVG ou PDF como fonte.
- Um commit deve ter escopo único, mensagem clara e indicar páginas/áreas alteradas quando houver mudança no diagrama.
- Não reordene, reformate ou regenere o XML inteiro sem necessidade; preserve diffs pequenos e auditáveis.
- Antes de publicar, abra o arquivo no draw.io e faça revisão visual. Para alterações de diagrama, use o **draw.io MCP** para abrir a página afetada, conferir que todos os elementos e conectores renderizam e confirmar que não há sobreposição, textos tocados por conectores ou página vazia; use `routing: "libavoid"` nessa revisão quando houver conectores.
- Registre na descrição da mudança quais páginas foram revisadas e se a revisão foi visual ou somente documental.

## 11. Validação XML

Antes de aceitar qualquer alteração em `fast.drawio`:

1. Confirme que existe exatamente um arquivo fonte `fast.drawio`.
2. Verifique que o arquivo é XML bem formado, com uma única raiz `mxfile` e páginas `diagram` válidas.
3. Confirme que cada célula tem ID único e que referências `source`/`target` apontam para células existentes.
4. Confirme que cada conector tem `mxGeometry` relativo e que não há comentários XML, caracteres não escapados ou XML truncado.
5. Abra o arquivo no draw.io MCP e faça a revisão visual descrita acima.

Falha em qualquer item bloqueia a publicação, mesmo que o arquivo pareça abrir no editor.

## 12. Regra final: não misturar abstrações

Antes de editar, declare mentalmente o nível C4 da página. Cada elemento, label, forma, relação e detalhe deve pertencer a esse nível. Se a informação responder a uma pergunta de outro nível, remova-a da página e crie/atualize a visão correspondente. Uma página mais simples e coerente é preferível a uma página completa, porém ambígua.
