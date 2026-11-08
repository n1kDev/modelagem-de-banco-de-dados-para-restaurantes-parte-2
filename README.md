# Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final

### Modelagem de um sistema de gestão de informações para uma organização de pequeno porte

> Este arquivo é o esqueleto dos artefatos finais do grupo, a serem organizados no repositório GitHub. **Será corrigido exclusivamente o conteúdo da branch `main` do repositório** — arquivos que estiverem em qualquer outra branch não serão considerados, mesmo que o link enviado aponte para ela. Preencha cada seção com o conteúdo do seu projeto. Alguns itens são documentos Markdown, outros são arquivos separados (diagramas, dicionário de dados, script `.sql`, massa de dados, slides) — está indicado em cada seção. Nesta etapa vocês atuam como uma **software house** implementando o banco de dados completo do sistema modelado na Entrega 1 e revisado na seção 1.1 — seja para uma empresa, uma ONG ou outra organização escolhida pelo grupo.

> **Lembrete:** termos como "empresa" e "negócio" usados abaixo são o vocabulário técnico padrão de modelagem de dados e se aplicam a qualquer tipo de organização, com ou sem fins lucrativos.

> **Importante:** esta etapa exige uma **organização real**, com evidências de existência e contato com um responsável.

> - **Se a organização da Entrega 1 era real**, continue com ela — não é permitido trocá-la por outra.  
> - **Se a organização da Entrega 1 era fictícia** (ou não teve a existência comprovada na correção), o grupo deve **procurar agora uma organização real** e refazer, para ela, toda a base da Entrega 1, inclusive o DER (ver seção 1.1, Caso B).

> Em qualquer caso, o grupo deve manter o acesso à organização, pois pode ser necessário voltar a campo para validar regras, tirar dúvidas sobre o processo de normalização ou conferir se a massa de dados reflete a realidade observada.

---

## 1\. Relatório Técnico Final

*(arquivo Markdown — reúne a maior parte da Dimensão Conceitual, 30%)*

**O Relatório Técnico Final deve ser o próprio `README.md` da raiz da branch `main`**, seguindo a estrutura deste esqueleto. Relatório entregue em outro arquivo (PDF, Word, outro `.md`, Google Docs, wiki do GitHub etc.) **não será corrigido**. Os demais artefatos (DER, modelo lógico, dicionário de dados, levantamento, script SQL, massa de dados, slides) ficam em arquivos separados no repositório, com link a partir do README.

### Integrantes

| Nome completo do aluno | RGM | Usuário do GitHub |
| :---- | :---- | :---- |
| Nome | RGM | usuario |

&nbsp;

### 1.1 Revisão do Modelo Conceitual

*(obrigatória, sem peso próprio — mas todo o restante da entrega é construído e avaliado sobre este modelo: um erro que permanecer no DER revisado se propaga para a conversão, a normalização, o modelo lógico e o script SQL)*

**Caracterização da organização (todos os grupos)**

*Repita aqui a caracterização da organização — atualizada, mesmo que a organização seja a da Entrega 1\. Na Entrega 1, os erros mais comuns foram: porte sem números, problema declarado que depois não aparecia no DER, evidências fora do repositório e modelo cobrindo só parte da organização. Evite-os:*

- **Nome e natureza da organização:** qual organização real o grupo escolheu (com acesso garantido para pesquisa de campo) — pode ser uma empresa (livraria, lanchonete, pet shop), uma ONG, uma associação comunitária ou outra instituição.  
- **Contexto e porte:** com ou sem fins lucrativos; tamanho da operação; número de pessoas envolvidas (funcionários, voluntários, membros, fiéis); volume de atividades (vendas, atendimentos, doações, rituais, eventos). **Informe números**, não só adjetivos (ex.: "cerca de 300 pedidos por semana", e não "grande movimento") — são eles que justificam o tamanho da massa de dados (seção 4).  
- **Problemas e necessidades identificados:** qual é a "crise operacional" — o que está desorganizado hoje (planilhas soltas, papel, falta de controle de estoque/doações/cadastros etc.)? Numere cada problema (`P1`, `P2`…). **Todo problema declarado aqui precisa ser resolvido pelo modelo:** deve aparecer no DER e ter ao menos uma consulta gerencial no script SQL (ver a tabela de rastreabilidade da seção 1.6). O problema deve ser de **gestão de informações** — refazer o site ou melhorar a divulgação da organização, por exemplo, não é um problema que um banco de dados resolva.  
- **Escopo do sistema:** o banco de dados deve reunir **todos os dados da organização**, não só os de um setor ou processo. Mesmo que os problemas declarados se concentrem em uma área, o modelo não pode servir somente a um recorte (ex.: só o estoque de uma clínica, deixando de fora pacientes e atendimentos).  
- **Justificativa da escolha:** por que essa organização foi escolhida e por que ela é um bom caso para o projeto? Se algum integrante trabalha na organização ou tem vínculo familiar com os responsáveis, declare isso — o vínculo não gera desconto, mas **não dispensa a comprovação da entrevista com o responsável, realizada no ambiente da organização**.  
- **Evidências da organização:** comprove que a organização existe e que o grupo teve acesso a ela — fotos do local/da visita, link da organização no Google, endereço completo e forma de contato. **Prefira o perfil da organização no Google Meu Negócio (Google Business Profile)**, que é a comprovação mais direta; site e redes sociais complementam. As fotos só são aceitas como **arquivos JPG ou PNG dentro do repositório** — fotos hospedadas fora dele (Google Drive, Imgur, anexos colados no editor do GitHub etc.) não são consideradas.  
- **Responsável pela organização:** **nome e telefone** de quem atendeu o grupo — o professor pode entrar em contato para confirmar as informações.

**Relato da entrevista (todos os grupos)**

*Relate a entrevista com o responsável pela organização. A entrevista da Entrega 1 pode ser reaproveitada se atender a todos os itens abaixo — inclusive ter sido feita no ambiente da organização, com foto — e se tiver levantado o necessário para os processos, requisitos e regras desta etapa. Se algo precisou ser confirmado depois (ex.: processos típicos levantados por pesquisa), relate também a nova visita.*

- **Quem:** nome e função do entrevistado e quais integrantes do grupo participaram.  
- **Quando e onde:** data e local — a entrevista deve ser realizada **no ambiente da organização**. A **observação presencial** da rotina é parte importante do levantamento de requisitos: o que o grupo vê acontecer no local (como um pedido é anotado, onde o estoque é controlado, que papéis e planilhas circulam) muitas vezes difere do que é dito na entrevista. **Destaque os achados obtidos pela observação**, separados dos obtidos pelas respostas do entrevistado.  
- **Comprovação:** ao menos uma foto da entrevista no local, como arquivo JPG ou PNG no repositório.  
- **O que foi levantado:** o que foi perguntado, o que foi confirmado e, se houve nova visita, o que mudou em relação à Entrega 1 (processos, regras, volumes, problemas). O levantamento completo fica em **um arquivo Markdown (`.md`) à parte no repositório**, com link no README — **recomenda-se fortemente** colocá-lo numa pasta chamada **`levantamento`** (ex.: `levantamento/levantamento.md`). Esse arquivo é um **texto organizado, escrito pelo grupo**, sobre o que foi levantado — **não** é um vídeo ou áudio da entrevista, nem a transcrição crua de formulários ou de perguntas e respostas. **No README, apresente um resumo do levantamento.**

**Nível de detalhe — processos, requisitos e regras de negócio (todos os grupos)**

*Frases genéricas como "o sistema deve permitir cadastrar cliente" são **superficiais** e não bastam. Em processos, requisitos e regras, indique **quais dados** estão envolvidos — todos os campos que aparecem no trabalho (DER, dicionário de dados, modelo lógico):*

| Superficial | Detalhado |
| :---- | :---- |
| *O sistema deve permitir cadastrar cliente.* | *RF03 — O sistema deve permitir cadastrar cliente com nome completo, CPF, telefone, e-mail e endereço (logradouro, número, bairro, CEP).* |
| *O cliente faz o pedido.* | *O garçom anota no pedido a mesa, o número da pulseira do cliente e, para cada item, o produto, a quantidade e as observações.* |
| *O cliente precisa estar cadastrado.* | *RN05 — Todo cliente deve ter CPF válido e único; telefone e nome completo são obrigatórios.* |

&nbsp;

**Processos de negócio (todos os grupos)**

*Apresente os **principais processos mapeados** na organização — ex.: cadastro de clientes/beneficiários/fiéis, controle de estoque ou doações, vendas ou arrecadação, emissão de pedidos ou solicitações, entregas ou distribuição, organização de eventos/rituais/mutirões.*

- **Levante os processos típicos do tipo de organização.** É recomendável usar pesquisas e ferramentas de IA para descobrir quais processos são comuns em organizações do mesmo tipo (ex.: o que toda pizzaria, ONG de doação de alimentos ou oficina mecânica costuma fazer) e conferir se eles existem na organização escolhida. Isso pode exigir **uma nova visita à organização** para confirmar como cada processo funciona lá. Registre esse uso de IA na seção 6\.  
- **Inclua os processos que existem na organização**, mesmo os que não estão ligados aos problemas declarados — o banco de dados reúne todos os dados da organização (ver *Escopo do sistema* acima).  
- **Justifique os processos prováveis não incluídos:** se um processo comum nesse tipo de organização ficou de fora (ex.: compras de fornecedores, cancelamentos, escala da equipe), explique por quê — por exemplo, a organização não o realiza, ou ele é feito por terceiros.

**Requisitos (todos os grupos)**

*Apresente os requisitos do sistema, atualizados com base na correção da Entrega 1 (ou elaborados agora, para quem trocou de organização):*

- **Requisitos Funcionais:** o que o sistema precisa **fazer**, com os dados envolvidos (ex.: "o sistema deve permitir registrar uma venda com data, cliente, vendedor, forma de pagamento e, para cada item, produto, quantidade e preço unitário"). Cada um com um código (`RF01`, `RF02`…).  
- **Requisitos Não Funcionais:** características de qualidade (ex.: desempenho, segurança, usabilidade, disponibilidade). Cada um com um código (`RNF01`, `RNF02`…).  
- **Levante os requisitos típicos do tipo de organização.** Assim como nos processos, é recomendável usar pesquisas e ferramentas de IA para descobrir quais requisitos funcionais e não funcionais são comuns em sistemas de organizações semelhantes e conferir se eles se aplicam à organização escolhida — o que pode exigir confirmação com o responsável. Registre esse uso de IA na seção 6\.  
- **Justifique os requisitos prováveis não incluídos:** se um requisito comum nesse tipo de organização ficou de fora, explique por quê (ex.: a organização não realiza o processo correspondente, ou a necessidade não existe no porte dela).

*Na Entrega 1, os erros mais comuns foram: requisitos de um sistema diferente do que foi mapeado (ex.: cardápio por QR Code ou motor de recomendação, quando o processo real é o garçom anotar o pedido), problemas declarados sem nenhum requisito que os resolva e requisitos técnicos (autenticação por JWT, hospedagem em nuvem) listados como funcionais. Os requisitos devem corresponder aos processos de negócio descritos acima e cobrir todos os problemas declarados (`P1`, `P2`…).*

---

### Regras de negócio

As regras abaixo vêm da entrevista no Ponto A e da observação no local. As que dependem de uma proposta do banco (RN06 e RN07) estão marcadas. Os nomes entre crases são as colunas do modelo.

#### Regras operacionais

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN01 | Toda visita começa com a entrega de uma pulseira numerada pelo segurança. O cliente não é cadastrado. | Visita: `data_hora_abertura` e `status` (aberta). Comanda: `codigo` e `status` (em uso). |
| RN02 | O número da pulseira vai de 0 a 9.999, em ordem crescente, e volta a zero depois do 9.999. Ele é único entre as comandas em uso. | Comanda: `codigo`. |
| RN03 | Se o cliente perde a pulseira, a comanda fica como perdida e a conta é localizada pelo garçom que tirou o pedido e pela mesa. | Comanda: `status` (perdida). Pedido: funcionário que atende. Visita: mesa. |
| RN04 | Todo consumo pertence a uma visita aberta. A visita só é fechada depois de a conta ser quitada, e a pulseira é baixada pelo caixa na saída. | Visita: `status` (fechada) e `data_hora_fechamento`. Comanda: `status` (baixada). |
| RN05 | Cada mesa pertence a um único ambiente (salão, deck ou subsolo). Uma visita pode ocupar mais de uma mesa. | Mesa: `numero` e ambiente. |
| RN06 | **(Proposta)** A entrada em um ambiente segue a ordem de chegada, conduzida pela gerência. Cada solicitação registra a visita, o ambiente, a hora do pedido de entrada e o número de pessoas. Toda entrada e toda saída são registradas. | Acesso: `data_hora_solicitacao`, `data_hora_entrada`, `data_hora_saida`, `quant_pessoas` e `status` (aguardando, no ambiente, encerrado ou desistiu). |
| RN07 | **(Proposta)** Um ambiente não recebe mais pessoas do que a sua capacidade. O que passar disso aguarda na fila. | Ambiente: `capacidade_maxima`. Acesso: `quant_pessoas` e `status`. |
| RN08 | Todos os ambientes vendem comida e bebida. Nenhuma categoria do cardápio é restrita a um ambiente. | Prato e categoria. |
| RN09 | O garçom registra o pedido no aparelho de mão. Todo pedido tem uma visita, um funcionário que atende, data e hora, e ao menos um item. | Pedido: `data_hora` e `status`. |
| RN10 | Cada item guarda o prato, a quantidade, as observações e o preço do momento do pedido. O preço do cardápio muda pouco, e o valor cobrado fica registrado no item mesmo se ele mudar depois. | Item: `qtd` (maior que zero), `preco_unitario` e `observacao`. |
| RN11 | Ao finalizar o pedido, cada item segue para o setor da categoria do prato. | Categoria: `setor_producao` (bar, cozinha, narguilé, comida japonesa ou pizzaria). |
| RN12 | Cada prato pertence a uma única categoria. | Prato e categoria. |
| RN13 | Só a gerência cancela, e o cancelamento é feito por item. O item cancelado permanece registrado e não conta como venda. | Item: `status` (ativo ou cancelado). |
| RN14 | Estornos e alterações em conta fechada são feitos só pela gerência. | Pagamento e visita. |

#### Pagamento e caixa

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN15 | A taxa de serviço é de 10% do consumo e é registrada no pagamento. | Pagamento: `valor_taxa_servico`. |
| RN16 | Todo pagamento tem nota fiscal, e o número da nota é registrado. A emissão é feita fora do banco. | Pagamento: `numero_nf` (único). |
| RN17 | A conta pode ser dividida em mais de um pagamento. A soma dos pagamentos da visita corresponde ao consumo mais a taxa. As formas aceitas são dinheiro, crédito, débito, pix e ticket. | Pagamento: `valor_total` (maior que zero) e `forma_pgto`. |
| RN18 | O garçom ou o caixa pode receber o pagamento, e todo pagamento é finalizado em um caixa. | Pagamento, funcionário e caixa. |
| RN19 | O caixa abre com R$ 150, em cada um dos dois turnos, e é fechado só pelo maître ou pela gerência. Não há sangria: valores externos são tratados com a administração. | Caixa: `valor_abertura`, `data_hora_abertura`, `valor_fechamento`, `data_hora_fechamento` e `status`. |

#### Estoque e compras

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN20 | A saída de bebida do estoque é registrada por requisição do bar, com a quantidade retirada. | Movimentação: `tipo` (saída), `qtd`, `data_hora`, produto e funcionário. |
| RN21 | O maître conta o estoque todos os dias. A divergência vira um ajuste positivo ou negativo com o motivo, e a quebra é o motivo mais comum. As movimentações nunca são apagadas nem alteradas. | Movimentação: `tipo` (ajuste positivo ou negativo), `qtd` e `motivo`. |
| RN22 | Cada produto tem um estoque mínimo, definido e acompanhado pelos maîtres. Quando a quantidade atual chega ao mínimo, o produto é sinalizado. | Produto: `qtd_atual` e `estoque_minimo`. |
| RN23 | Só a bebida vendida como vem (garrafa ou lata) se liga a um produto de estoque. Drinks e doses são feitos com as garrafas disponíveis e não baixam o estoque por receita. | Prato e produto de estoque. |
| RN24 | A compra é feita pelo gerente geral a um fornecedor cadastrado e tem ao menos um item. | Fornecedor: `razao_social`, `cnpj` e `telefone`. Compra: `data_compra`, `valor_total` e `status` (pedida). Item: produto, `qtd` e `custo_unitario`. |
| RN25 | A mercadoria é recebida e conferida pelos maîtres. No recebimento, a compra passa a recebida e as entradas no estoque ficam ligadas a ela. A compra cancelada permanece registrada. | Compra: `data_recebimento` e `status` (recebida ou cancelada). Movimentação: `tipo` (entrada). |

#### Restrições legais e organizacionais

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN26 | Bebida alcoólica só é vendida a maiores de idade. O garçom confere a idade em documento físico ou carteira digital, e a idade não é registrada. | Nenhum dado do cliente é guardado. |
| RN27 | O banco não guarda dados pessoais de clientes. O único dado pessoal é o nome do funcionário, com acesso restrito à gerência e à auditoria, e toda consulta fica no log de acesso. | Funcionário: `nome_func`. |
| RN28 | O acesso ao sistema depende do cargo: cada setor tem a sua aba, com um nível de acesso diferente. | Funcionário: `cargo`. |
| RN29 | Toda operação identifica o funcionário que a fez: pedido, pagamento, movimentação de estoque, compra e caixa. | Funcionário: `id_func` e `cargo`. |

#### Regras prováveis não incluídas

| Regra comum em restaurantes | Por que ficou de fora |
|---|---|
| Reserva de mesas | O responsável informou que as reservas são tratadas manualmente pela administração, fora do sistema de atendimento. |
| Escala e folha de pagamento | A administração trata manualmente. O funcionário entra no modelo só para registrar quem fez cada operação e o seu cargo. |
| Registros sanitários e de vigilância | A casa mantém esses registros com a administração, fora do sistema. O estoque controla só quantidades, sem lote nem validade. |
| Emissão de nota fiscal | A emissão é feita por sistema fiscal externo. O banco guarda só o número da nota no pagamento. |
| Cadastro de clientes, fidelidade e LGPD sobre dados de clientes | A casa não cadastra clientes e não tem pontos nem níveis. O cliente é identificado só pela pulseira. |
| Delivery e retirada | O delivery é feito por aplicativos de terceiros, fora do sistema da casa. |
| Ficha técnica (insumos por prato) | O controle é de bebidas. Os drinks não baixam o estoque por receita. |
| Sangria do caixa | Não existe no Ponto A. Valores externos são tratados com a administração. |
| Cobrança pela perda da pulseira | A entrevista não indicou cobrança. A casa só localiza a conta pelo garçom e pela mesa. |
| Horário de funcionamento, áreas restritas aos funcionários e normas de conduta | São políticas da casa que orientam a operação e não geram dado próprio para registrar. |

---



#### DER conceitual revisado (Caso A)
 
*DER conceitual revisado do Ponto A*
 
**Software utilizado:** BRModelo Web, na notação do brModelo (entidades em retângulos, relacionamentos em losangos, atributos em círculos, chaves em círculos preenchidos e cardinalidades mínima e máxima nos dois lados de cada relacionamento). Arquivo: [DER.jpg](DER.jpg).
 
O DER revisado tem **19 entidades** e **24 relacionamentos**. Ele cobre o atendimento (do cliente que chega até o pagamento) e os dois problemas declarados: o controle de acesso aos ambientes (P1) e o controle de estoque de bebidas (P2). Também cobre o caixa e as compras de insumos. Não há chaves estrangeiras no DER; elas aparecem apenas no modelo lógico.
 
##### Entidades e atributos
 
Os atributos em **negrito** são os identificadores (chaves primárias).
 
| Entidade | Atributos |
|---|---|
| CLIENTE | **id_cliente**, nome_cliente, cpf_cliente, data_nasc, telefone |
| MESA | **id_mesa**, numero |
| AMBIENTE | **id_ambiente**, nome_ambiente, capacidade_maxima, status |
| CONTROLE_ACESSO | **id_controle_acesso**, data_hora_solicitacao, data_hora_entrada, data_hora_saida, quant_pessoas, status |
| VISITA | **id_visita**, data_hora_abertura, data_hora_fechamento, status |
| COMANDA | **id_comanda**, codigo, status |
| PAGAMENTO | **id_pgto**, valor_total, valor_taxa_servico, forma_pgto, data_hora, numero_nf |
| FUNCIONARIO | **id_func**, nome_func, cargo |
| PEDIDO | **id_pedido**, status, data_hora, data_hora_entrega |
| ITEM_PEDIDO | **id_item**, qtd, preco_unitario, observacao |
| PRATO | **id_prato**, nome_prato, preco |
| CATEGORIA_PRATO | **id_categ**, nome, setor_producao |
| PRODUTO_ESTOQUE | **id_produto**, nome, qtd_atual, estoque_minimo |
| MOVIMENTACAO_ESTOQUE | **id_movimentacao**, tipo, qtd, data_hora |
| FORNECEDOR | **id_fornecedor**, razao_social, cnpj, telefone |
| COMPRA | **id_compra**, data_compra, data_recebimento, valor_total, status |
| ITEM_COMPRA | **id_item_compra**, qtd, custo_unitario |
| CAIXA | **id_caixa**, data_hora_abertura, valor_abertura, data_hora_fechamento, valor_fechamento, status |
| SANGRIA | **id_sangria**, data_hora, valor, motivo |
 
##### Relacionamentos e cardinalidades
 
O par (mín,máx) ao lado de cada entidade indica quantas vezes ela participa do relacionamento.
 
| Relacionamento | Entidade A | Card. A | Card. B | Entidade B | Leitura |
|---|---|---|---|---|---|
| Realiza | CLIENTE | (0,n) | (1,n) | VISITA | Um cliente pode ter várias visitas, e uma visita reúne um ou mais clientes (grupo) |
| Recebe | MESA | (0,n) | (1,n) | VISITA | Uma mesa recebe várias visitas, e uma visita pode ocupar mais de uma mesa |
| Possui | AMBIENTE | (1,n) | (1,1) | MESA | Cada mesa pertence a um único ambiente |
| Controla | AMBIENTE | (0,n) | (1,1) | CONTROLE_ACESSO | Cada registro de acesso é de um único ambiente |
| Solicita | VISITA | (0,n) | (1,1) | CONTROLE_ACESSO | Cada registro de acesso (solicitação, entrada e saída) é de uma única visita |
| Identifica | VISITA | (1,1) | (1,1) | COMANDA | Cada visita tem uma comanda (pulseira) e cada comanda identifica uma visita |
| Gera | VISITA | (1,n) | (1,1) | PEDIDO | Uma visita gera vários pedidos, e cada pedido pertence a uma visita |
| Quita | VISITA | (1,n) | (1,1) | PAGAMENTO | Uma visita é quitada por um ou mais pagamentos (divisão de conta) |
| Atende | FUNCIONARIO | (1,n) | (1,1) | PEDIDO | Cada pedido é atendido por um único funcionário |
| Arrecada | FUNCIONARIO | (1,n) | (1,n) | PAGAMENTO | O garçom cobra e o caixa finaliza: mais de um funcionário por pagamento |
| Contem | PEDIDO | (1,n) | (1,1) | ITEM_PEDIDO | Todo pedido tem ao menos um item, e cada item pertence a um pedido |
| Refere-se a | PRATO | (0,n) | (1,1) | ITEM_PEDIDO | Cada item se refere a um único prato |
| Classifica | CATEGORIA_PRATO | (1,n) | (1,n) | PRATO | Um prato pode estar em mais de uma categoria |
| Corresponde a | PRATO | (0,1) | (0,n) | PRODUTO_ESTOQUE | Só bebida vendida como vem (lata, garrafa) corresponde a um produto de estoque |
| Executa | FUNCIONARIO | (0,n) | (1,1) | MOVIMENTACAO_ESTOQUE | Cada movimentação é feita por um funcionário |
| Movimenta | PRODUTO_ESTOQUE | (0,n) | (1,1) | MOVIMENTACAO_ESTOQUE | Cada movimentação é de um único produto |
| Abastece | COMPRA | (0,n) | (0,1) | MOVIMENTACAO_ESTOQUE | A compra recebida origina as movimentações de entrada |
| Efetua | FUNCIONARIO | (0,n) | (1,1) | COMPRA | Cada compra é feita por um funcionário |
| Fornece | FORNECEDOR | (0,n) | (1,1) | COMPRA | Cada compra é de um único fornecedor |
| Inclui | COMPRA | (1,n) | (1,1) | ITEM_COMPRA | Toda compra tem ao menos um item |
| É comprado em | PRODUTO_ESTOQUE | (0,n) | (1,1) | ITEM_COMPRA | Cada item de compra é de um único produto |
| Opera | FUNCIONARIO | (0,n) | (1,1) | CAIXA | Cada turno de caixa é operado por um funcionário |
| Recebe | CAIXA | (0,n) | (1,1) | PAGAMENTO | Todo pagamento é finalizado em um caixa |
| Sofre | CAIXA | (0,n) | (1,1) | SANGRIA | Cada sangria é de um único caixa |
 
##### Revisão do modelo (Caso A)
 
A tabela registra cada apontamento da correção da Entrega 1 e o que foi feito.
 
| Apontamento da correção | O que foi feito | Por quê |
|---|---|---|
| `contém` (PEDIDO–PRATO) N:N sem quantidade nem preço praticado | Criada a entidade ITEM_PEDIDO com `qtd`, `preco_unitario` e `observacao`, ligada a PEDIDO (`contem`) e a PRATO (`refere-se a`) | Sem ela não é possível saber quanto de cada prato foi pedido nem o preço cobrado na época |
| PRATO sem nome | Acrescentado `nome_prato` | O cardápio precisa identificar cada prato |
| Sem ambiente, embora o acesso ao Deck seja problema declarado (P1) | Criadas AMBIENTE (`capacidade_maxima`) e CONTROLE_ACESSO (solicitação, entrada, saída, `quant_pessoas`, `status`) | Permite calcular a lotação e o tempo de espera de cada ambiente |
| Sem estoque, embora o controle de bebidas seja problema declarado (P2) | Criadas PRODUTO_ESTOQUE (`qtd_atual`, `estoque_minimo`) e MOVIMENTACAO_ESTOQUE (`tipo`, `qtd`, `data_hora`), com o relacionamento `corresponde a` ligando bebida do cardápio ao produto | Registra toda entrada, saída e ajuste (conferência) e permite sinalizar estoque abaixo do mínimo |
| Caixa (abertura, sangria, fechamento) ausente | Criadas CAIXA e SANGRIA, e o relacionamento `recebe` entre CAIXA e PAGAMENTO | O processo existe no Ponto A e encerra a visita |
| Compra e recebimento de mercadoria, fornecedor ausentes | Criadas FORNECEDOR, COMPRA e ITEM_COMPRA, ligadas ao estoque por `abastece` e `é comprado em` | Garante que a compra seja de fornecedor cadastrado e que a entrada no estoque seja rastreável |
| Cancelamento de pedido ausente | Mantido no atributo `status` de PEDIDO ("cancelado"); o pedido permanece no banco | Cancelamento exige histórico, não exclusão |
| Taxa de serviço, cupom fiscal e perda da pulseira sem registro | Acrescentados `valor_taxa_servico` e `numero_nf` em PAGAMENTO e `status` em COMANDA (livre, em uso ou perdida) | Permitem aplicar e auditar essas regras |
| Atributos sobrepostos em COMANDA_ACESSO | Entidade redesenhada como COMANDA, com `id_comanda`, `codigo` e `status` legíveis | Falha de legibilidade do desenho |
| Mínimos (1,n) impediam cliente sem visita e mesa nunca usada | CLIENTE e MESA passaram a (0,n) nos relacionamentos `Realiza` e `Recebe` | O banco deve admitir cliente cadastrado ainda sem visita e mesa ainda sem uso |
| Reservas, escala da equipe, normas sanitárias e horário de funcionamento ausentes | **Mantidos fora do modelo.** As reservas são feitas pelos próprios funcionários e serão tratadas em uma etapa futura; os demais itens são informações de gestão de pessoal e de conformidade, que não fazem parte do fluxo interno de atendimento, consumo, caixa e estoque que o banco controla | Justificativa de processo não incluído |
 
##### Decisões de modelagem
 
- **VISITA é o eixo do modelo.** Pedidos, pagamentos, comanda e acessos aos ambientes partem dela, e é ela que liga clientes e mesas (N:N, para grupos e mesas unidas).
- **FUNCIONARIO é uma única entidade**, com o atributo `cargo` (garçom, caixa, produção ou gerente), porque todos os papéis compartilham os mesmos atributos e relacionamentos.
- **ITEM_PEDIDO e ITEM_COMPRA** são entidades próprias, com atributos, no lugar de relacionamentos N:N sem dados, para guardar quantidade e preço praticado de cada item.
- **Os N:N que permanecem** (CLIENTE–VISITA, MESA–VISITA, FUNCIONARIO–PAGAMENTO e CATEGORIA_PRATO–PRATO) viram tabelas associativas no modelo lógico.

---

### 1.2 Conversão do Modelo Conceitual para o Modelo Lógico

*(vale 7,5%)*

- **Entidades → Tabelas:** *como cada entidade virou tabela.*  
- **Atributos → Colunas:** *conversão correta dos atributos.*  
- **Relacionamentos → Chaves:** *como os relacionamentos foram traduzidos.*  
- **Cardinalidades:** *como cada cardinalidade (1:1, 1:N, N:N) foi tratada (inclusive tabelas associativas, se houver).*  
- **Generalização/especialização:** *se o DER tiver, qual estratégia de conversão foi usada (tabela única, uma tabela por entidade ou só as especializadas) e por quê.*

### 1.3 Definição de Chaves Primárias e Estrangeiras

*Liste as PKs e FKs de cada tabela (serve de base para a justificativa técnica abaixo).*

### 1.4 Processo de Normalização (1FN, 2FN e 3FN)

*(vale 7,5%)*

- **1FN:** *eliminação de grupos repetitivos/atributos multivalorados.*  
- **2FN:** *eliminação de dependências parciais.*  
- **3FN:** *eliminação de dependências transitivas.*  
- **Justificativa do processo:** *por que cada forma normal foi necessária no seu caso.*

### 1.5 Justificativas Técnicas das Decisões

*(vale 7,5%)*

- Justificar as **chaves primárias** escolhidas.  
- Justificar as **chaves estrangeiras** e os relacionamentos que elas implementam.  
- Justificar as **restrições** aplicadas (unicidade, obrigatoriedade, valores permitidos etc.), citando o código da regra de negócio (`RNxx`) que cada restrição implementa, quando houver.  
- Justificar **decisões arquiteturais** gerais do modelo.

---

### 1.6 Proposta de Arquitetura Analítica para BI e IA

### Informações estratégicas

- Em quais ambientes e horários há lotação, fila ou ociosidade, e quanto o cliente espera para entrar no Deck, para decidir equipe e layout.
- Risco de ruptura de estoque, consumo médio de bebidas e diferenças entre as saídas de estoque e as vendas.
- Ticket médio e faturamento por ambiente, período e forma de pagamento.
- Itens cancelados e o consumo de cada visita e de cada mesa.
- Gasto com compras por fornecedor e prazo de recebimento da mercadoria.
- Diferenças de caixa no fechamento e pagamentos sem nota fiscal.

A gerência do Ponto A pediu para acompanhar ruptura de estoque, cancelamentos, ticket médio e tempo de espera do Deck. Esses quatro assuntos correspondem aos KPIs K4, K8, K7 e K2, que são os principais. O responsável também pediu um informativo mais detalhado sobre o consumo de cada mesa (K16) e prefere relatórios mensais, por isso todas as consultas podem ser agrupadas por mês.

### Indicadores e métricas gerenciais (KPIs)

| Cód. | KPI | Fórmula (tabelas do DER) | Período | Meta proposta |
|---|---|---|---|---|
| K1 | Taxa de ocupação por ambiente | `quant_pessoas` (CONTROLE_ACESSO) dos acessos com entrada e sem saída ÷ `capacidade_maxima` (AMBIENTE) × 100 | hora / dia | ≤ 100% (ideal de 70% a 90%) |
| K2 | Tempo médio de espera na fila | média de (`data_hora_entrada` − `data_hora_solicitacao`) em CONTROLE_ACESSO, só nos acessos com entrada | dia / mês | ≤ 15 min |
| K3 | Taxa de desistência da fila | acessos com status "desistiu" ÷ total de acessos solicitados × 100 (CONTROLE_ACESSO) | semana / mês | ≤ 10% |
| K4 | Produtos abaixo do estoque mínimo | nº de produtos com `qtd_atual` ≤ `estoque_minimo` (PRODUTO_ESTOQUE) | dia | 0 |
| K5 | Taxa de ajustes de estoque | soma da `qtd` das movimentações de ajuste ÷ soma da `qtd` de todas as movimentações × 100 (MOVIMENTACAO_ESTOQUE) | mês | ≤ 2% |
| K6 | Consumo médio diário de bebidas | soma da `qtd` das movimentações de saída ÷ dias do período, por produto (MOVIMENTACAO_ESTOQUE, PRODUTO_ESTOQUE) | semana / mês | comparar com o histórico |
| K7 | Ticket médio por ambiente | soma do `valor_total` dos pagamentos ÷ nº de visitas pagas. O ambiente vem da mesa da visita; se a visita usa mesas de ambientes diferentes, vale o ambiente da mesa de menor `id_mesa` | dia / mês | a definir com o responsável |
| K8 | Taxa de cancelamento de itens | itens com status "cancelado" ÷ total de itens × 100 (ITEM_PEDIDO) | semana / mês | ≤ 3% |
| K9 | Diferença entre saídas de estoque e vendas | saídas registradas (MOVIMENTACAO_ESTOQUE) − quantidade vendida do produto, contando só itens ativos de pratos ligados ao produto, ou seja, garrafa e lata (ITEM_PEDIDO, PRATO, PRODUTO_ESTOQUE) | dia / semana | 0 |
| K10 | Faturamento por ambiente e hora | soma do `valor_total` dos pagamentos por ambiente e hora do pagamento. Mesma regra de ambiente do K7 | dia / mês | comparar com o histórico |
| K11 | Pagamentos sem número de nota fiscal | pagamentos com `numero_nf` vazio ÷ total de pagamentos × 100 (PAGAMENTO) | mês | 0% |
| K12 | Diferença de caixa no fechamento | `valor_fechamento` − (`valor_abertura` + pagamentos em dinheiro do caixa), por caixa (CAIXA, PAGAMENTO) | turno / dia | 0 |
| K13 | Gasto de compras por fornecedor | soma de `qtd` × `custo_unitario` (ITEM_COMPRA) por fornecedor, sem compras canceladas (COMPRA, FORNECEDOR) | mês | comparar com o orçamento |
| K14 | Prazo médio de recebimento da mercadoria | média de (`data_recebimento` − `data_compra`) por fornecedor, só nas compras recebidas (COMPRA) | mês | a definir |
| K15 | Faturamento por período e forma de pagamento | soma do `valor_total` por `forma_pgto` e por dia ou mês (PAGAMENTO) | dia / mês | comparar com o histórico |
| K16 | Consumo por visita e mesa | soma de `qtd` × `preco_unitario` dos itens ativos, por visita e por mesa (ITEM_PEDIDO, PEDIDO, VISITA, MESA) | visita / mês | sem meta (informativo) |

As metas são propostas do grupo, a confirmar com o responsável: o tempo de espera do Deck não é medido hoje. Uma consulta de auditoria confere que o `valor_total` de cada compra é igual à soma dos seus itens, para que o K13 seja confiável. O K9 só cobre bebidas vendidas como vêm (garrafa e lata), porque os drinks e as doses não baixam o estoque por receita.

### Potencial dos dados para BI

- **Painel de ocupação:** lotação, fila e tempo de espera por ambiente (K1 a K3).
- **Painel de estoque:** produtos abaixo do mínimo, ajustes, consumo e diferenças entre saídas e vendas (K4 a K6, K9).
- **Painel financeiro:** faturamento, ticket médio, horários de pico e forma de pagamento (K7, K10, K15).
- **Painel de compras:** gasto e prazo de recebimento por fornecedor (K13, K14).
- **Painel de caixa e conformidade:** diferença de caixa e pagamentos sem nota (K11, K12).
- **Painel de atendimento:** itens cancelados e consumo por visita e mesa (K8, K16).

### Aplicações analíticas e de IA

- Previsão de demanda por ambiente e horário (base: CONTROLE_ACESSO e VISITA), para dimensionar a equipe. A casa recebe cerca de 500 pessoas por dia útil e 1.000 em fim de semana.
- Previsão de consumo e reposição de bebidas (base: MOVIMENTACAO_ESTOQUE), indicando quando cada produto chega ao estoque mínimo.
- Previsão de compras considerando o prazo de recebimento de cada fornecedor.
- Detecção de anomalias em itens cancelados, quebras e ajustes de estoque, diferenças entre saídas e vendas (K9) e diferenças de caixa (K12).
- Análise de cesta (itens que aparecem juntos em ITEM_PEDIDO), para apoiar decisões de cardápio.
- Estimativa do tempo de fila esperado por ambiente e horário (base: CONTROLE_ACESSO), para apoiar a gerência no planejamento da equipe.

### Rastreabilidade

| Problema ou KPI | Tabelas envolvidas | Consulta no script |
|---|---|---|
| P1: acesso ao Deck | AMBIENTE, CONTROLE_ACESSO | G01, G02, G03 |
| P2: estoque de bebidas | PRODUTO_ESTOQUE, MOVIMENTACAO_ESTOQUE, PRATO, ITEM_PEDIDO | G04, G05, G06, G09 |
| K1: taxa de ocupação | AMBIENTE, CONTROLE_ACESSO | G01 |
| K2: tempo médio de espera | CONTROLE_ACESSO | G02 |
| K3: taxa de desistência | CONTROLE_ACESSO | G03 |
| K4: produtos abaixo do mínimo | PRODUTO_ESTOQUE | G04 |
| K5: taxa de ajustes de estoque | MOVIMENTACAO_ESTOQUE | G05 |
| K6: consumo médio de bebidas | MOVIMENTACAO_ESTOQUE, PRODUTO_ESTOQUE | G06 |
| K7: ticket médio por ambiente | PAGAMENTO, VISITA, MESA, AMBIENTE | G07 |
| K8: taxa de cancelamento de itens | ITEM_PEDIDO | G08 |
| K9: saídas de estoque x vendas | MOVIMENTACAO_ESTOQUE, ITEM_PEDIDO, PRATO, PRODUTO_ESTOQUE | G09 |
| K10: faturamento por ambiente e hora | PAGAMENTO, VISITA, MESA, AMBIENTE | G10 |
| K11: pagamentos sem nota fiscal | PAGAMENTO | G11 |
| K12: diferença de caixa | CAIXA, PAGAMENTO | G12 |
| K13: gasto de compras por fornecedor | COMPRA, ITEM_COMPRA, FORNECEDOR | G13 |
| K14: prazo de recebimento | COMPRA, FORNECEDOR | G14 |
| K15: faturamento por forma de pagamento | PAGAMENTO | G15 |
| K16: consumo por visita e mesa | ITEM_PEDIDO, PEDIDO, VISITA, MESA | G16 |

---

## 2\. Modelo Lógico Relacional

*(diagrama separado — vale 10% na Dimensão Procedimental)*

O modelo lógico deve seguir a **notação visual do brModelo** para o modelo lógico, como no exemplo abaixo. Informe no README **em qual software ou aplicativo ele foi criado**.

![Exemplo de modelo lógico na notação do brModelo](Exemplo_Logico.png)

| Elemento | Como aparece | No exemplo |
| :---- | :---- | :---- |
| **Tabela** | retângulo com o nome da tabela no topo e uma linha por coluna | `Entity 1`, `Entity 5` |
| **Coluna** | nome da coluna, à esquerda, com o tipo de dado à direita | `Attribute 30` — `INT` |
| **Chave primária** | marca `PK` ao lado do tipo | `Chave 1`, em `Entity 1`; `Chave 50` \+ `Chave 51`, em `Entity 5` (chave primária composta) |
| **Chave estrangeira** | marca `FK` ao lado do tipo | `Chave 3`, em `Entity 4` |
| **Coluna que é chave primária e estrangeira** | marcas `PK, FK` | `Chave 1`, em `Entity 2` |
| **Ligação entre tabelas** | linha entre a tabela da FK e a tabela referenciada, com a cardinalidade `(mín, máx)` em cada ponta | `Entity 3` — `Entity 4`: `(1, 1)` e `(0, n)` |

&nbsp;

Deve apresentar claramente:

- Tabelas  
- Atributos (colunas), com seus tipos de dados  
- Chaves primárias  
- Chaves estrangeiras  
- Cardinalidades já convertidas

As **demais restrições de integridade** — obrigatoriedade (`NOT NULL`), unicidade (`UNIQUE`), valores permitidos (`CHECK`), valores padrão (`DEFAULT`) — não aparecem no diagrama do brModelo e devem ser documentadas no **dicionário de dados** (seção 5\) e implementadas no **script SQL** (seção 3).

---

## 3\. Script SQL Completo

*(arquivo `.sql` — vale 12% na Dimensão Procedimental)*

O script completo é **um arquivo no formato `.sql` disponível no repositório**, de preferência numa pasta chamada **`docs`** (ex.: `docs/script.sql`), com link no README. Script colado só no README, em PDF, em imagem ou em outro formato não é aceito.

**SGBD:** MySQL — o disponível no laboratório ou outra instalação equivalente a que o grupo tenha acesso. O script deve executar no MySQL.

O script deve conter, nesta ordem lógica:

1. `CREATE DATABASE`  
2. `CREATE TABLE` (todas as tabelas, com tipos de dados adequados)  
3. **Constraints** (PK, FK, `NOT NULL`, `UNIQUE`, `CHECK` etc.)  
4. `INSERT INTO` (carga da massa de dados — ver seção 4\)  
5. `SELECT` com `JOIN` (consultas que cruzam múltiplas tabelas)  
6. `UPDATE`  
7. `DELETE`  
8. **Consultas gerenciais** (que respondem perguntas de gestão, ex.: faturamento/arrecadação por período, número de atendimentos/beneficiários)  
9. **Consultas de auditoria** (que verificam consistência/integridade dos dados)

Cada **consulta gerencial** e cada **consulta de auditoria** deve ter um cabeçalho em comentário com um código (`G01`, `G02`… para gerenciais; `A01`, `A02`… para auditoria), a pergunta que ela responde em português e o que ela atende:

```sql
-- G03 — Quais bebidas estão abaixo do estoque mínimo hoje?
-- Atende: P1 (ruptura de estoque de bebidas)
SELECT ...

-- A02 — Existe pedido registrado sem nenhum item?
-- Verifica: RN04 (todo pedido tem ao menos um item)
SELECT ...
```

&nbsp;

- **Consultas gerenciais** respondem aos problemas declarados e aos KPIs da seção 1.6 — pelo menos uma por problema declarado e por KPI.  
- **Consultas de auditoria** verificam regras de negócio, citando o código da regra (`RNxx`) que cada uma confere.  
- Essas mesmas consultas são as usadas na demonstração da apresentação final (seção 7).

*Teste o script do zero (banco vazio) antes de entregar, para garantir que ele executa sem erros.*

---

## 4\. Massa de Dados

*(incluída no script ou em arquivo separado — parte da nota de Implementação SQL, 12%)*

Se a massa de dados ficar em arquivo separado, ele deve seguir a mesma regra do script: **um arquivo no formato `.sql` no repositório**, de preferência na pasta **`docs`** (ex.: `docs/massa_de_dados.sql`), com link no README.

- **Dados reais da organização (nomes de clientes, fiéis, beneficiários, doadores, valores reais etc.) não devem ser utilizados, em respeito à privacidade dessas informações.** A massa de dados deve ser **fictícia**, mas **coerente com as operações modeladas** (mesmos tipos de processo, volumes e regras observados na organização real).  
- Volume de registros **coerente com a realidade da organização** (nada de 3 linhas de exemplo — mas também nada artificialmente inflado sem sentido).  
- **Explique como o volume real da organização** — os números informados em *Contexto e porte* (seção 1.1), como clientes ativos, pedidos por mês, atendimentos por semana, itens no estoque — **justifica a quantidade de registros de cada tabela** da massa de dados.  
- Dados consistentes o suficiente para validar os processos organizacionais modelados (ex.: pedidos que referenciam clientes e produtos existentes, ou doações que referenciam doadores e campanhas existentes).  
- **Variedade e quantidade coerentes com a organização, para que os testes sejam possíveis:** os dados devem cobrir as situações reais — não só o caso comum, mas também pedidos cancelados, clientes sem compras, produtos abaixo do estoque mínimo, pagamentos em formas diferentes, registros em períodos diferentes. Sem essa variedade, as consultas gerenciais e de auditoria (seção 3\) não têm o que mostrar: uma consulta de produtos abaixo do estoque mínimo que retorna vazio não demonstra nada.  
- **Documente o uso de LLMs** para gerar essa massa de dados na seção 6 (**Uso de Inteligência Artificial**), incluindo ferramentas, prompts e ajustes feitos manualmente.

---

## 5\. Repositório GitHub

*(vale 13% — Documentação e Repositório)*

O repositório deve conter:

- Relatório Técnico (no `README.md`)  
- **Dicionário de Dados** atualizado, no modelo do arquivo `02-03g_Exemplo_Dicionario_Dados.pdf` e com as restrições de integridade de cada coluna (obrigatoriedade, unicidade, valores permitidos, valor padrão), em **um único arquivo HTML** no repositório. **Exceção — turma de GTI:** o dicionário é apresentado em **um único arquivo Markdown (`.md`) separado do README**, seguindo o mesmo modelo do `02-03g_Exemplo_Dicionario_Dados.pdf`.  
- Diagramas (DER revisado e modelo lógico)  
- Levantamento (pasta `levantamento`, recomendada) e evidências da organização e da entrevista (fotos JPG ou PNG)  
- Script SQL completo em arquivo `.sql` (de preferência na pasta `docs`)  
- **Histórico de commits** que evidencie o desenvolvimento ao longo do tempo (evite um único commit final)  
- **Evidências de colaboração entre os membros do grupo:** é a listagem de commits do repositório, que deve estar **equilibrada entre os integrantes** — a quantidade de commits deve se distribuir de forma proporcional entre todos os membros do grupo, cada um fazendo commits com a própria conta do GitHub. O integrante precisa ser **identificável na conta do GitHub**: o nome configurado na conta deve ser compatível com o nome do aluno na tabela de Integrantes, onde também deve constar o usuário do GitHub de cada um

---

## 6\. Uso de Inteligência Artificial

*(documentação obrigatória — não é opcional se o grupo usou IA em qualquer etapa: pesquisa, levantamento de processos, requisitos e regras típicos, escrita, organização de ideias, revisão de texto, geração de massa de dados ou apoio a scripts SQL)*

Para cada uso relevante de ferramenta de IA (ChatGPT, Claude, Gemini, Perplexity etc.), registre:

| Item | O que registrar |
| :---- | :---- |
| **Ferramenta e etapa** | Qual IA foi usada e em qual parte do trabalho (ex.: normalização, geração da massa de dados, escrita de queries SQL, redação do relatório, preparação dos slides). |
| **Motivação** | Por que o grupo recorreu à IA nesse ponto específico. |
| **Prompt(s) utilizados** | Texto exato (ou muito próximo) do que foi perguntado/pedido à IA. |
| **Resposta recebida** | Resumo ou trecho relevante da resposta da IA. |
| **Fontes consultadas e verificadas** | Se a IA citou fontes/dados, quais foram checadas pelo grupo e como. |
| **Trechos rejeitados ou corrigidos** | O que da resposta da IA foi descartado, editado ou corrigido manualmente (ex.: query SQL ajustada, dado fictício alterado), e por quê. |
| **Justificativa da escolha final** | Por que o grupo manteve, adaptou ou rejeitou o que a IA sugeriu. |
| **Reflexão crítica** | Limites, vieses ou erros identificados no uso da IA nessa etapa (ex.: normalização incorreta sugerida pela IA, script SQL com erro de sintaxe, dado fictício incoerente com o volume real da organização). |

&nbsp;

*Se o grupo não usou nenhuma ferramenta de IA em alguma dessas etapas, declare isso explicitamente.*

**Uso de IA observado e não declarado será penalizado.**

---

## 7\. Apresentação Corporativa Final

*(pitch cronometrado de **8 minutos** — vale 15%, maior peso individual da Dimensão Procedimental)*

Formato: slides com estética limpa e minimalista — **proibida a leitura de blocos longos de texto**.

O pitch deve conter, nesta ordem:

1. **Contextualização do problema institucional/organizacional** solucionado  
2. **Apresentação da solução** proposta  
3. **Apresentação do modelo lógico**  
4. **Demonstração da implementação** (SQL rodando)  
5. **Demonstração de consultas** (gerenciais/auditoria)  
6. **Potencial para BI e IA**  
7. **Benefícios diretos** para a tomada de decisão da organização

**Defesa do modelo lógico:** logo após o pitch, fora do tempo cronometrado, o grupo responde às perguntas do professor e da turma. **Cada pergunta deve ser respondida exclusivamente pelo aluno a quem foi dirigida** — os colegas não podem responder por ele.

**Regras da apresentação:**

- **Participação:** não é necessário e nem recomendável que todos os integrantes falem durante o pitch, mas é **obrigatório que todos participem** da apresentação e estejam presentes para a defesa.  
- **Ausência:** o integrante ausente na apresentação terá **nota reduzida**, exceto em casos com justificativa legal (ex.: doença que impeça o comparecimento, com atestado).  
- **Apresentação pessoal:** será avaliada. Postura e vestimenta adequadas a uma apresentação corporativa. Não são adequados bonés, bermudas, chinelos, camisas de time, roupas esportivas ou semelhantes.

---

## Critérios Atitudinais (20%)

**Estes critérios NÃO constam explicitamente como item de entrega nos artefatos do grupo.** Eles são avaliados por meio de **Avaliação 360º entre os integrantes do grupo** (cada membro avalia os colegas de equipe) e, no caso da colaboração, também pela **colaboração equilibrada no histórico de commits** do repositório GitHub (Seção 5\) e pela conduta do grupo na **Apresentação Corporativa Final** (seção 7\) — não pela leitura do relatório:

- **Participação e colaboração (5%):** participação técnica ativa \+ colaboração com o grupo, incluindo colaboração equilibrada no histórico de commits do repositório GitHub.  
- **Responsabilidade (5%):** cumprimento de prazos \+ entrega dos itens sob sua responsabilidade.  
- **Postura profissional (5%):** comunicação profissional \+ conduta ética.  
- **Autonomia e resolução de problemas (5%):** buscar soluções independentes \+ propor melhorias.

---

## Resumo dos Pesos

| Dimensão | Peso total | Itens principais |
| :---- | :---- | :---- |
| Conceitual | 30% | Conversão conceitual→lógico (7,5%), Normalização (7,5%), Justificativas técnicas (7,5%), Análise BI/IA (7,5%) |
| Procedimental | 50% | Modelo lógico (10%), Implementação SQL (12%), Documentação/repositório (13%), Apresentação final (15%) |
| Atitudinal | 20% | Participação/colaboração, responsabilidade, postura profissional, autonomia |

&nbsp;

**Entrega final:** Relatório Técnico \+ Dicionário de Dados \+ Modelo Conceitual \+ Modelo Lógico \+ Script SQL \+ Massa de Dados \+ Repositório GitHub, seguidos da Apresentação Corporativa Final perante o professor e a turma.

&nbsp;
