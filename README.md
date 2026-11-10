# **Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final**



## 1\. Relatório Técnico Final

### Modelagem de um sistema de gestão de informações para um restaurante

### Integrantes

| Nome completo do aluno | RGM | Usuário do GitHub |
| :---- | :---- | :---- |
| Diego Xavier Ribeiro | 47793007 | ytiow-die |
| Luiz Felipe de Brito Carvalho | 47857714 | felipeBrito-369 |
| Nicolas Vieira de Lima | 47212021 | n1kDev |
| Renan Caio de Lima | 46915575 | SnyQ285 |

---

## 1.1 Revisão do Modelo Conceitual
 
**Caracterização da organização**
 
**Nome e natureza.** Ponto A – Restaurante e Pizzaria, restaurante real com fins lucrativos, na Rua Dante Pellacani, 192, Vila Regente Feijó, São Paulo (SP). A casa tem salão, deck e subsolo, e vende pratos de cozinha, pizzas, comida japonesa, bebidas e narguilé.
 
**Contexto e porte.** Números informados pelo responsável na entrevista:
 
| Indicador | Valor |
|---|---|
| Funcionários | de 60 a 70 (67 na soma dos cargos listados) |
| Funcionamento | de segunda a segunda, em dois turnos |
| Pessoas atendidas por dia | cerca de 500 em dia útil e 1.000 em fim de semana e feriado |
| Pedidos por dia | de 2.000 a 5.000 |
| Pedidos atípicos | cerca de 10 por quinzena (garrafas premium) |
| Mesas | 55, com 4 lugares cada: 22 no salão, 13 no deck e 20 no subsolo |
| Itens do cardápio | de 400 a 800, em 5 categorias (bar, cozinha, narguilé, comida japonesa e pizzaria) |
| Fornecedores | de 40 a 60, com entregas de segunda a sábado |
 
A equipe tem garçons (11), bar (6), caixa (3), valet (5), cozinha (15), narguilé (3), limpeza (4), sushiman (2), pizzaiolo (2), segurança (5), cumim (4), RH (1), maître (3) e gerente (3). O pedido é feito por aparelho de mão (Palm) e a casa usa hoje o sistema xMenu.
 
**Problemas e necessidades identificados.**
 
- **P1 – Acesso aos ambientes (Deck).** A entrada no Deck nos horários de pico é por ordem de chegada, conduzida pela gerência. Nada é registrado: não há horário de solicitação, de entrada nem de saída, e o tempo de espera não é medido. A gerência não sabe quanto o cliente espera nem quanto cada ambiente está ocupado.
- **P2 – Controle de estoque de bebidas.** A saída é registrada por requisição do bar, e o pedido só dá uma quantidade aproximada do que saiu. O xMenu não direciona o funcionário na contagem nem controla as doses. A contagem é feita pelos maîtres, que também definem o estoque mínimo, com uma base diária. A quebra é a divergência mais comum.
Os dois problemas foram confirmados pelo responsável como os que a casa quer resolver: "estoque e triagem de mesa".
 
**Escopo do sistema.** O banco reúne os dados de todo o atendimento: pulseira e visita, mesas e ambientes, controle de acesso, pedidos e itens, pagamentos e caixa, cardápio, estoque, compras e fornecedores, e funcionários. Ficam fora do modelo as reservas, a escala da equipe e os registros sanitários, que a administração trata manualmente fora do sistema de atendimento.
 
**Justificativa da escolha.** O Ponto A tem volume alto, vários processos ligados entre si (acesso, pedido, estoque, compra e caixa) e dois problemas de gestão de informação que um banco de dados resolve. Um dos integrantes do grupo trabalha no Ponto A e convive diariamente com esses problemas, o que facilitou o acesso à casa e deu origem à ideia do projeto. Esse vínculo não dispensa a entrevista com o responsável, que foi feita no local.
 
**Evidências da organização.**
 
- Perfil no Google: [Perfil no maps](https://maps.app.goo.gl/XJJXeGhYZD4S7TNm9)
- Endereço: Rua Dante Pellacani, 192 - Vila Reg. Feijó, São Paulo - SP, 03334-070
- Contato: [site da casa](https://ponto-a.cluvi.com.br), Fone: 01126722292
- Foto da visita: [foto](evidencias/fotoEntrevista.jpg)
**Responsável:** Luiz Eduardo Almeida, maître do Ponto A, telefone 11 91101-7123.
 
---

### Relato da entrevista
 
- **Quem:** Luiz Eduardo Almeida, maître. Entrevistador: Renan Caio de Lima. Nenhum outro integrante estava presente.
- **Quando e onde:** 06/11/2026, no Ponto A – Restaurante e Pizzaria, no ambiente da organização.
- **Comprovação:** foto da entrevista em [evidencia](evidencias/Fotoentrevista2.jpg).
- **O que foi levantado.** A entrevista teve 39 perguntas sobre porte, atendimento, controle de acesso, estoque, compras, caixa, regras e indicadores. Ela confirmou os dois problemas declarados e trouxe os volumes da casa. Também mudou o modelo em sete pontos, entre eles: o cliente não é cadastrado e só a pulseira o identifica, não há sangria no caixa, cada prato fica em uma só categoria e o cancelamento é feito pela gerência, item a item. A lista completa está em "Mudanças feitas após a entrevista", na revisão do DER. O levantamento completo está em [levantamento/levantamento.md](levantamento/levantamento.md).
**Observação presencial** (o que o grupo viu, separado das respostas):
 
- O pedido sai do aparelho do garçom e o sistema envia uma comanda a cada setor.
- Os papéis e sistemas da rotina ficam no escritório.
- A pulseira é conferida e recolhida na saída, no fechamento da conta, pelo caixa.
- A fila e a entrada no Deck seguem a ordem de chegada.
- As bebidas saem do estoque quando o responsável do bar informa a quantidade retirada.

---

### Processos de negócio

Os processos abaixo foram mapeados na entrevista e na observação no Ponto A. A coluna "Situação" diz se o processo já existe na casa ou se é uma proposta do banco para resolver um problema declarado. Os nomes entre crases são as colunas do modelo.

| Cód. | Processo | Quem executa | Dados envolvidos | Situação |
|---|---|---|---|---|
| PR01 | **Entrega da pulseira e abertura da visita.** O cliente chega sem cadastro e recebe uma pulseira numerada. | Segurança | Comanda: `codigo` (de 0 a 9.999, em ordem, voltando a zero depois do 9.999) e `status` (em uso). Visita: `data_hora_abertura` e `status` (aberta). Mesa(s) ocupada(s). | Existe |
| PR02 | **Controle de acesso aos ambientes (P1).** Hoje a fila do Deck é por ordem de chegada, sem registro. O banco passa a registrar a solicitação, a entrada e a saída de cada visita. | Gerência | Acesso: visita, ambiente, `data_hora_solicitacao`, `data_hora_entrada`, `data_hora_saida`, `quant_pessoas` e `status` (aguardando, no ambiente, encerrado ou desistiu). Ambiente: `capacidade_maxima`. | Proposto |
| PR03 | **Registro do pedido.** O garçom anota no aparelho de mão e o sistema envia uma comanda a cada setor. | Garçom | Pedido: visita, funcionário que atende, `data_hora` e `status`. Item: prato, `qtd`, `preco_unitario` (preço do momento) e `observacao` (ex.: ponto da carne). | Existe |
| PR04 | **Preparo e envio aos setores.** Cada setor recebe a sua comanda e prepara os itens. | Bar, cozinha, narguilé, sushiman e pizzaiolo | Setor de produção da categoria do prato (`setor_producao`) e `status` do pedido (registrado, em preparo, pronto, entregue). | Existe |
| PR05 | **Cancelamento de item.** Só a gerência cancela, e o cancelamento sobe ao sistema com a comanda do setor. O item permanece registrado. | Gerência | Item: `status` (ativo ou cancelado). | Existe |
| PR06 | **Pagamento e fechamento da conta.** A conta pode ser dividida em vários pagamentos, e o garçom pode receber na mesa. Ao final, o caixa dá baixa na pulseira. Estornos e alterações em conta fechada são só da gerência. | Garçom, caixa e gerência | Pagamento: `valor_total`, `valor_taxa_servico` (10%), `forma_pgto` (dinheiro, crédito, débito, pix ou ticket), `data_hora` e `numero_nf`. Visita: `data_hora_fechamento` e `status` (fechada). Comanda: `status` (baixada). | Existe |
| PR07 | **Abertura e fechamento do caixa.** Cada turno abre com R$ 150. Não há sangria. | Caixa abre; maître ou gerência fecha | Caixa: `data_hora_abertura`, `valor_abertura`, `data_hora_fechamento`, `valor_fechamento`, `status` e funcionário responsável. | Existe |
| PR08 | **Controle de estoque de bebidas (P2).** A saída é registrada por requisição do bar. O maître conta o estoque todo dia, lança os ajustes (a quebra é o mais comum) e acompanha o estoque mínimo. | Bar e maître | Movimentação: produto, funcionário, `tipo` (entrada, saída, ajuste positivo ou negativo), `qtd`, `data_hora` e `motivo`. Produto: `qtd_atual` e `estoque_minimo`. | Existe, com controle manual e sem direcionar a contagem |
| PR09 | **Compra e recebimento de mercadoria.** O gerente geral compra com base nas informações dos setores. Os maîtres recebem e conferem a entrega. | Gerente e maître | Fornecedor: `razao_social`, `cnpj` e `telefone`. Compra: `data_compra`, `valor_total`, `status` (pedida, recebida ou cancelada) e `data_recebimento`. Item: produto, `qtd` e `custo_unitario`. As entradas de estoque ficam ligadas à compra. | Existe |
| PR10 | **Manutenção dos cadastros de apoio.** Cardápio (prato, `preco`, categoria), mesas e ambientes, produtos de estoque, fornecedores e funcionários (`cargo`). | Gerência | Prato: `nome_prato`, `preco` e categoria. Mesa: `numero` e ambiente. Funcionário: `nome_func` e `cargo`. | Existe |

#### Processos prováveis não incluídos

| Processo comum em restaurantes | Por que ficou de fora |
|---|---|
| Reserva de mesas | O responsável informou que as reservas são tratadas manualmente pela administração, fora do sistema de atendimento. |
| Escala e folha de pagamento da equipe | São tratadas manualmente pela administração. O funcionário entra no modelo só para registrar quem fez cada operação e o seu cargo. |
| Registros sanitários e de vigilância | A casa precisa manter esses registros, mas eles ficam com a administração e não fazem parte do sistema de atendimento. |
| Emissão da nota fiscal | A nota é emitida por sistema fiscal próprio. O banco guarda só o número da nota em cada pagamento. |
| Cadastro de clientes e programa de fidelidade | A casa não cadastra clientes: o cliente é identificado só pela pulseira, e dados de clientes não são guardados. |
| Ficha técnica (insumos de cada prato) | O controle de estoque é de bebidas. Os drinks são feitos com as garrafas disponíveis, sem baixa por receita. |
| Sangria do caixa | Não existe no Ponto A: valores externos são tratados com a administração. |
| Delivery e retirada | O delivery é feito por aplicativos de terceiros, fora do sistema da casa. O modelo cobre o atendimento presencial no salão, no deck e no subsolo. |
| Serviço de valet | É um serviço de apoio, sem relação com o atendimento, o estoque ou o caixa, e não gera dados do modelo. O valet aparece só como cargo de funcionário. |

---

### Requisitos funcionais

Cada requisito indica o processo que ele atende (entre parênteses) e os dados envolvidos. Os problemas declarados são o **P1** (controle de acesso aos ambientes e triagem de mesas) e o **P2** (controle de estoque de bebidas).

**Atendimento e acesso**

- **RF01** — O sistema deve permitir abrir uma visita quando a pulseira é entregue, registrando o número da pulseira (de 0 a 9.999), a situação "em uso" e a data e hora de abertura, **sem pedir nenhum dado do cliente**. Deve impedir abrir um número de pulseira que já esteja em uso, porque a numeração volta a zero depois do 9.999. *(PR01)*
- **RF02** — O sistema deve permitir ligar a visita a uma ou mais mesas e consultar as mesas ocupadas, com número da mesa, ambiente, número da pulseira, hora de abertura da visita e consumo acumulado. *(PR01 — P1)*
- **RF03** — O sistema deve permitir registrar o pedido de entrada de uma visita em um ambiente, com ambiente, data e hora da solicitação, número de pessoas e a situação "aguardando", e listar a fila de cada ambiente por ordem de chegada. *(PR02 — P1)*
- **RF04** — O sistema deve permitir registrar a entrada (data e hora), a saída (data e hora) e a desistência de cada pedido de acesso, e mostrar a ocupação atual de cada ambiente comparada à sua capacidade máxima. *(PR02 — P1)*

**Pedidos**

- **RF05** — O sistema deve permitir registrar um pedido com a visita, o garçom, a data e hora e, para cada item, o prato ou bebida, a quantidade, o preço do momento e as observações (por exemplo, o ponto da carne). *(PR03)*
- **RF06** — O sistema deve enviar os itens do pedido ao setor de produção da categoria de cada prato (bar, cozinha, narguilé, comida japonesa ou pizzaria), listar por setor o que há para preparar e permitir acompanhar a situação do pedido (registrado, em preparo, pronto ou entregue). *(PR04)*
- **RF07** — O sistema deve permitir que **somente a gerência** cancele um item do pedido. O item cancelado continua registrado e deixa de contar na conta. *(PR05)*

**Conta, pagamento e caixa**

- **RF08** — O sistema deve calcular a conta da visita: a soma dos itens não cancelados (quantidade × preço do momento) mais 10% de taxa de serviço. *(PR06)*
- **RF09** — O sistema deve permitir registrar um ou mais pagamentos para a mesma visita (divisão de conta), com valor, taxa de serviço, forma de pagamento (dinheiro, crédito, débito, pix ou ticket), data e hora, número da nota fiscal, o caixa e o funcionário que recebeu (caixa ou garçom, na mesa). *(PR06)*
- **RF10** — O sistema deve fechar a visita quando a conta estiver quitada, registrando a data e hora de fechamento, e baixar a pulseira, liberando o número para reutilização. *(PR06)*
- **RF11** — O sistema deve permitir marcar a pulseira como perdida e localizar a conta pelo garçom que tirou o pedido e pela mesa. *(PR06)*
- **RF12** — O sistema deve permitir que **somente a gerência** estorne ou altere um pagamento de conta fechada, sem apagar o registro do pagamento. *(PR06)*
- **RF13** — O sistema deve permitir abrir o caixa com data e hora, valor de abertura (R$ 150) e operador, e fechá-lo com data e hora e valor de fechamento, **somente por maître ou gerência**. No fechamento, deve mostrar o total recebido por forma de pagamento e a diferença em relação ao valor esperado. *(PR07)*

**Estoque e compras**

- **RF14** — O sistema deve permitir registrar a saída de bebidas por requisição do bar, com produto, quantidade, data e hora e funcionário responsável, atualizando a quantidade atual do produto. *(PR08 — P2)*
- **RF15** — O sistema deve permitir registrar o resultado da contagem diária e as quebras como ajustes (positivo ou negativo), com produto, quantidade, motivo, data e hora e o maître responsável, atualizando a quantidade atual. *(PR08 — P2)*
- **RF16** — O sistema deve listar, todos os dias, os produtos cuja quantidade atual está igual ou abaixo do estoque mínimo. *(PR08 — P2)*
- **RF17** — O sistema deve emitir, por produto e período, o comparativo **vendido × retirado**: as unidades vendidas por garrafa ou lata (itens não cancelados), as saídas por requisição do bar, os ajustes e a diferença entre eles. *(PR08 — P2)*
- **RF18** — O sistema deve permitir registrar uma compra com fornecedor, gerente que comprou, data da compra e, para cada item, produto, quantidade e custo unitário, calculando o valor total. *(PR09)*
- **RF19** — O sistema deve permitir registrar o recebimento da compra, com data de recebimento, gerando uma entrada no estoque para cada item, ligada à compra, e atualizando a quantidade atual. *(PR09)*

**Cadastros de apoio**

- **RF20** — O sistema deve permitir cadastrar categorias do cardápio (nome e setor de produção) e pratos e bebidas (nome, preço e **uma única** categoria). A bebida vendida como vem (garrafa ou lata) deve poder ser ligada ao seu produto de estoque. *(PR10)*
- **RF21** — O sistema deve permitir cadastrar ambientes (nome, capacidade máxima e situação) e mesas (número e ambiente). *(PR10)*
- **RF22** — O sistema deve permitir cadastrar funcionários com nome e cargo, escolhido em uma lista fixa que define o acesso ao sistema. *(PR10)*
- **RF23** — O sistema deve permitir cadastrar produtos de estoque (nome, quantidade atual e estoque mínimo) e fornecedores (razão social, CNPJ e telefone). *(PR10)*

**Relatórios gerenciais (mensais)**

- **RF24** — O sistema deve emitir o relatório de cancelamentos: quantidade e valor dos itens cancelados, por setor e por garçom, e a taxa de cancelamento sobre o total de itens. *(PR05)*
- **RF25** — O sistema deve emitir o ticket médio: valor pago dividido pelo número de visitas pagas, por mês e por ambiente. *(PR06)*
- **RF26** — O sistema deve emitir o tempo médio e o maior tempo de espera no Deck (entrada menos solicitação), por mês, dia da semana e faixa de horário, e o número de desistências. *(PR02 — P1)*
- **RF27** — O sistema deve emitir o relatório de ruptura: para cada produto, os dias em que a quantidade ficou abaixo do estoque mínimo ou zerada. *(PR08 — P2)*
- **RF28** — O sistema deve emitir o consumo de cada visita e de cada mesa, que atende o pedido da gerência por um informativo mais detalhado sobre cada mesa. *(PR03, PR06 — P1)*

### Requisitos não funcionais

*Os números abaixo são metas propostas pelo grupo a partir dos volumes informados na entrevista. O responsável não os citou.*

- **RNF01 — Desempenho:** registrar um pedido e consultar a fila do Deck, as mesas ocupadas, a conta da visita e os produtos abaixo do mínimo deve levar até 2 segundos, nos picos de 2.000 a 5.000 pedidos por dia e com garçons, caixas, bar e gerência usando o sistema ao mesmo tempo. Os relatórios mensais podem demorar mais.
- **RNF02 — Disponibilidade:** a casa funciona de segunda a segunda, em dois turnos. O sistema deve estar disponível durante todo o funcionamento, e a manutenção deve ser feita fora dele.
- **RNF03 — Acesso por cargo:** cada funcionário entra com login próprio e vê só as funções do seu cargo, como já acontece no xMenu (uma aba por setor):
  - **Segurança:** pulseira e controle de acesso.
  - **Garçom:** pedidos, pedidos de acesso e pagamento na mesa.
  - **Produção (bar, cozinha, narguilé, sushiman e pizzaiolo):** situação dos pedidos do seu setor.
  - **Caixa:** contas e pagamentos.
  - **Bar:** saídas de estoque.
  - **Maître:** contagem, quebras, recebimento de mercadoria e fechamento de caixa.
  - **Gerência:** tudo o que os demais fazem, mais cancelamentos, estornos, compras, cadastros e relatórios.
- **RNF04 — Histórico:** pedidos, itens, pagamentos, movimentações de estoque, compras e caixas não são apagados. Cancelamentos, estornos e ajustes mudam a situação do registro e ficam com o responsável e a data e hora.
- **RNF05 — LGPD:** o sistema não coleta nome, CPF, telefone nem data de nascimento de clientes, mantendo a prática da casa. A idade continua sendo conferida em documento, sem registro. Os dados de funcionários se limitam a nome e cargo, com acesso restrito à gerência e ao RH.
- **RNF06 — Integridade e concorrência:** o registro do pedido com seus itens, o fechamento da conta e o recebimento de uma compra devem ser gravados por inteiro ou não ser gravados. Dois usuários não podem abrir a mesma pulseira em uso nem quitar a mesma conta duas vezes.
- **RNF07 — Backup:** cópia completa diária, feita fora do horário de maior movimento, para que uma falha não apague pedidos, pagamentos ou estoque.
- **RNF08 — Dia operacional:** como o funcionamento passa da meia-noite, os relatórios diários e o caixa devem seguir o turno e um horário de corte definido pela casa, e não o dia do calendário.
- **RNF09 — Volume:** o banco deve comportar cerca de 1,8 milhão de pedidos por ano (5.000 por dia), além de 400 a 800 itens de cardápio e 40 a 60 fornecedores.
- **RNF10 — Usabilidade no aparelho de mão:** o garçom deve buscar o item por nome ou categoria e escrever observações livres, sem digitar nenhum dado do cliente.

#### Requisitos prováveis não incluídos

| Requisito comum em sistemas de restaurante e bar | Por que ficou de fora |
|---|---|
| Cardápio por QR Code, aplicativo do cliente ou totem de autoatendimento | O pedido é anotado pelo garçom no aparelho de mão. Não há autoatendimento. |
| Reserva de mesas | As reservas são tratadas manualmente pela administração, fora do sistema. |
| Escala e ponto da equipe | São tratados manualmente pela administração. |
| Cadastro de clientes, fidelidade e recomendação | Nenhum dado de cliente é guardado, e isso iria contra o RNF05. |
| Registro digital da verificação de idade | A idade é conferida no documento, sem registro. Guardar o dado criaria tratamento de dado pessoal sem necessidade. |
| Emissão da nota fiscal | A nota sai de um sistema fiscal próprio. O banco guarda só o número em cada pagamento (RF09). |
| Sangria de caixa | Não existe: valores externos são tratados com a administração. |
| Registros sanitários e de vigilância | Ficam com a administração, fora do sistema de atendimento. |
| Financeiro (contas a pagar, folha, DRE) | É tratado pela administração. |
| Delivery e integração com aplicativos de entrega | É feito por aplicativos de terceiros, fora do sistema da casa. |
| Controle do serviço de valet | É um serviço de apoio, sem relação com atendimento, estoque ou caixa. |
| Baixa automática de estoque por ficha técnica de drinks e doses | Os drinks são feitos com as garrafas disponíveis, sem separação. O controle possível é o comparativo vendido × retirado (RF17). |
| Bloqueio automático de item esgotado | Depende da baixa automática, que não existe. O estoque mínimo é conferido todo dia pelos maîtres. |
| Tempo de preparo por item nos setores | A casa não mede o tempo de preparo. O banco guarda só a situação do pedido (RF06). |

#### Cobertura dos problemas e dos indicadores pedidos

| Problema ou indicador | Requisitos | Processos |
|---|---|---|
| **P1** — Controle de acesso e triagem de mesas | RF02, RF03, RF04, RF21, RF26, RF28 | PR01, PR02 |
| **P2** — Controle de estoque de bebidas | RF14, RF15, RF16, RF17, RF19, RF23, RF27 | PR08, PR09 |
| Ruptura de estoque | RF16, RF27 | PR08 |
| Cancelamentos | RF07, RF24 | PR05 |
| Ticket médio | RF08, RF09, RF25 | PR06 |
| Tempo de espera no Deck | RF03, RF04, RF26 | PR02 |
---


### Regras de negócio

As regras abaixo vêm da entrevista no Ponto A e da observação no local. As que dependem de uma proposta do banco (RN06 e RN07) estão marcadas. Os nomes entre crases são as colunas do modelo.

**Regras operacionais**

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

**Pagamento e caixa**

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN15 | A taxa de serviço é de 10% do consumo e é registrada no pagamento. | Pagamento: `valor_taxa_servico`. |
| RN16 | Todo pagamento tem nota fiscal, e o número da nota é registrado. A emissão é feita fora do banco. | Pagamento: `numero_nf` (único). |
| RN17 | A conta pode ser dividida em mais de um pagamento. A soma dos pagamentos da visita corresponde ao consumo mais a taxa. As formas aceitas são dinheiro, crédito, débito, pix e ticket. | Pagamento: `valor_total` (maior que zero) e `forma_pgto`. |
| RN18 | O garçom ou o caixa pode receber o pagamento, e todo pagamento é finalizado em um caixa. | Pagamento, funcionário e caixa. |
| RN19 | O caixa abre com R$ 150, em cada um dos dois turnos, e é fechado só pelo maître ou pela gerência. Não há sangria: valores externos são tratados com a administração. | Caixa: `valor_abertura`, `data_hora_abertura`, `valor_fechamento`, `data_hora_fechamento` e `status`. |

**Estoque e compras**

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN20 | A saída de bebida do estoque é registrada por requisição do bar, com a quantidade retirada. | Movimentação: `tipo` (saída), `qtd`, `data_hora`, produto e funcionário. |
| RN21 | O maître conta o estoque todos os dias. A divergência vira um ajuste positivo ou negativo com o motivo, e a quebra é o motivo mais comum. As movimentações nunca são apagadas nem alteradas. | Movimentação: `tipo` (ajuste positivo ou negativo), `qtd` e `motivo`. |
| RN22 | Cada produto tem um estoque mínimo, definido e acompanhado pelos maîtres. Quando a quantidade atual chega ao mínimo, o produto é sinalizado. | Produto: `qtd_atual` e `estoque_minimo`. |
| RN23 | Só a bebida vendida como vem (garrafa ou lata) se liga a um produto de estoque. Drinks e doses são feitos com as garrafas disponíveis e não baixam o estoque por receita. | Prato e produto de estoque. |
| RN24 | A compra é feita pelo gerente geral a um fornecedor cadastrado e tem ao menos um item. | Fornecedor: `razao_social`, `cnpj` e `telefone`. Compra: `data_compra`, `valor_total` e `status` (pedida). Item: produto, `qtd` e `custo_unitario`. |
| RN25 | A mercadoria é recebida e conferida pelos maîtres. No recebimento, a compra passa a recebida e as entradas no estoque ficam ligadas a ela. A compra cancelada permanece registrada. | Compra: `data_recebimento` e `status` (recebida ou cancelada). Movimentação: `tipo` (entrada). |

**Restrições legais e organizacionais**

| Cód. | Regra | Dados envolvidos |
|---|---|---|
| RN26 | Bebida alcoólica só é vendida a maiores de idade. O garçom confere a idade em documento físico ou carteira digital, e a idade não é registrada. | Nenhum dado do cliente é guardado. |
| RN27 | O banco não guarda dados pessoais de clientes. O único dado pessoal é o nome do funcionário, com acesso restrito à gerência e à auditoria, e toda consulta fica no log de acesso. | Funcionário: `nome_func`. |
| RN28 | O acesso ao sistema depende do cargo: cada setor tem a sua aba, com um nível de acesso diferente. | Funcionário: `cargo`. |
| RN29 | Toda operação identifica o funcionário que a fez: pedido, pagamento, movimentação de estoque, compra e caixa. | Funcionário: `id_func` e `cargo`. |

**Regras prováveis não incluídas**

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

### DER conceitual revisado (Caso A)



**Software utilizado:** BRModelo Web, na notação do brModelo (entidades em retângulos, relacionamentos em losangos, atributos em círculos, chaves em círculos preenchidos e cardinalidades mínima e máxima nos dois lados de cada relacionamento). Arquivo: [DER.jpg](DER.jpg).

O DER revisado tem **17 entidades** e **22 relacionamentos**. Ele cobre o atendimento (da pulseira entregue na chegada até o pagamento) e os dois problemas declarados: o controle de acesso aos ambientes (P1) e o controle de estoque de bebidas (P2). Também cobre o caixa e as compras de insumos. Não há chaves estrangeiras no DER; elas aparecem apenas no modelo lógico.

---
#### Entidades e atributos

Os atributos em **negrito** são os identificadores (chaves primárias).

| Entidade | Atributos |
|---|---|
| MESA | **id_mesa**, numero |
| AMBIENTE | **id_ambiente**, nome_ambiente, capacidade_maxima, status |
| CONTROLE_ACESSO | **id_controle_acesso**, data_hora_solicitacao, data_hora_entrada, data_hora_saida, quant_pessoas, status |
| VISITA | **id_visita**, data_hora_abertura, data_hora_fechamento, status |
| COMANDA | **id_comanda**, codigo, status |
| PAGAMENTO | **id_pgto**, valor_total, valor_taxa_servico, forma_pgto, data_hora, numero_nf |
| FUNCIONARIO | **id_func**, nome_func, cargo |
| PEDIDO | **id_pedido**, status, data_hora |
| ITEM_PEDIDO | **id_item**, qtd, preco_unitario, observacao, status |
| PRATO | **id_prato**, nome_prato, preco |
| CATEGORIA_PRATO | **id_categ**, nome, setor_producao |
| PRODUTO_ESTOQUE | **id_produto**, nome, qtd_atual, estoque_minimo |
| MOVIMENTACAO_ESTOQUE | **id_movimentacao**, tipo, qtd, data_hora, motivo |
| FORNECEDOR | **id_fornecedor**, razao_social, cnpj, telefone |
| COMPRA | **id_compra**, data_compra, data_recebimento, valor_total, status |
| ITEM_COMPRA | **id_item_compra**, qtd, custo_unitario |
| CAIXA | **id_caixa**, data_hora_abertura, valor_abertura, data_hora_fechamento, valor_fechamento, status |

---
#### Relacionamentos e cardinalidades

O par (mín,máx) ao lado de cada entidade indica quantas vezes ela participa do relacionamento.

| Relacionamento | Entidade A | Card. A | Card. B | Entidade B | Leitura |
|---|---|---|---|---|---|
| Ocupa | MESA | (0,n) | (1,n) | VISITA | Uma mesa é ocupada por várias visitas ao longo do tempo, e uma visita pode ocupar mais de uma mesa |
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
| Classifica | CATEGORIA_PRATO | (1,n) | (1,1) | PRATO | Cada prato pertence a uma única categoria |
| Corresponde a | PRATO | (0,1) | (0,n) | PRODUTO_ESTOQUE | Só bebida vendida como vem (lata, garrafa) corresponde a um produto de estoque |
| Executa | FUNCIONARIO | (0,n) | (1,1) | MOVIMENTACAO_ESTOQUE | Cada movimentação é feita por um funcionário |
| Movimenta | PRODUTO_ESTOQUE | (0,n) | (1,1) | MOVIMENTACAO_ESTOQUE | Cada movimentação é de um único produto |
| Abastece | COMPRA | (0,n) | (0,1) | MOVIMENTACAO_ESTOQUE | A compra recebida origina as movimentações de entrada |
| Efetua | FUNCIONARIO | (0,n) | (1,1) | COMPRA | Cada compra é feita por um funcionário |
| Fornece | FORNECEDOR | (0,n) | (1,1) | COMPRA | Cada compra é de um único fornecedor |
| Inclui | COMPRA | (1,n) | (1,1) | ITEM_COMPRA | Toda compra tem ao menos um item |
| É comprado em | PRODUTO_ESTOQUE | (0,n) | (1,1) | ITEM_COMPRA | Cada item de compra é de um único produto |
| Opera | FUNCIONARIO | (0,n) | (1,1) | CAIXA | Cada turno de caixa é operado por um funcionário |
| Registra | CAIXA | (0,n) | (1,1) | PAGAMENTO | Todo pagamento é finalizado e registrado em um caixa |

---
#### Revisão do modelo (Caso A)

A tabela registra cada apontamento da correção da Entrega 1 e o que foi feito.

| Apontamento da correção | O que foi feito | Por quê |
|---|---|---|
| `contém` (PEDIDO–PRATO) N:N sem quantidade nem preço praticado | Criada a entidade ITEM_PEDIDO com `qtd`, `preco_unitario` e `observacao`, ligada a PEDIDO (`contem`) e a PRATO (`refere-se a`) | Sem ela não é possível saber quanto de cada prato foi pedido nem o preço cobrado na época |
| PRATO sem nome | Acrescentado `nome_prato` | O cardápio precisa identificar cada prato |
| Sem ambiente, embora o acesso ao Deck seja problema declarado (P1) | Criadas AMBIENTE (`capacidade_maxima`) e CONTROLE_ACESSO (solicitação, entrada, saída, `quant_pessoas`, `status`) | Permite calcular a lotação e o tempo de espera de cada ambiente |
| Sem estoque, embora o controle de bebidas seja problema declarado (P2) | Criadas PRODUTO_ESTOQUE (`qtd_atual`, `estoque_minimo`) e MOVIMENTACAO_ESTOQUE (`tipo`, `qtd`, `data_hora`, `motivo`), com o relacionamento `corresponde a` ligando bebida do cardápio ao produto | Registra toda entrada, saída e ajuste (conferência) e permite sinalizar estoque abaixo do mínimo |
| Caixa (abertura e fechamento) ausente | Criada a entidade CAIXA e o relacionamento `registra` entre CAIXA e PAGAMENTO | O processo existe no Ponto A e encerra a visita |
| Compra e recebimento de mercadoria, fornecedor ausentes | Criadas FORNECEDOR, COMPRA e ITEM_COMPRA, ligadas ao estoque por `abastece` e `é comprado em` | Garante que a compra seja de fornecedor cadastrado e que a entrada no estoque seja rastreável |
| Cancelamento ausente | Registrado no atributo `status` de ITEM_PEDIDO ("cancelado"); o item permanece no banco | Cancelamento exige histórico, não exclusão, e no Ponto A ele é feito por item |
| Taxa de serviço, cupom fiscal e perda da pulseira sem registro | Acrescentados `valor_taxa_servico` e `numero_nf` em PAGAMENTO e `status` em COMANDA (em uso, baixada ou perdida) | Permitem aplicar e auditar essas regras |
| Atributos sobrepostos em COMANDA_ACESSO | Entidade redesenhada como COMANDA, com `id_comanda`, `codigo` e `status` legíveis | Falha de legibilidade do desenho |
| Mínimo (1,n) impedia mesa nunca usada | MESA passou a (0,n) no relacionamento `Ocupa` | O banco deve admitir mesa ainda sem uso |
| Reservas, escala da equipe, normas sanitárias e horário de funcionamento ausentes | **Mantidos fora do modelo.** O responsável confirmou que reservas, escala e registros sanitários são tratados manualmente pela administração, fora do sistema de atendimento | Justificativa de processo não incluído |

---

#### Mudanças feitas após a entrevista

A entrevista com o responsável no Ponto A confirmou a maior parte do modelo e levou a estes ajustes.

| O que o responsável informou | O que mudou no DER | Por quê |
|---|---|---|
| O cliente não é cadastrado: é reconhecido só pelo número da pulseira, e dados de clientes não são guardados | Removidos CLIENTE e o relacionamento `Realiza`; a visita se liga à comanda e à mesa | O modelo deve refletir o que a casa faz e não guardar dados pessoais de clientes |
| Não há sangria; valores externos são tratados com a administração | Removidos SANGRIA e o relacionamento `Sofre` | O processo não existe no Ponto A |
| Cada prato fica em uma única categoria | `Classifica` passou de N:N para 1:N | Evita uma tabela de ligação que a casa não usa |
| O cancelamento é feito só pela gerência, por item | `status` em ITEM_PEDIDO; PEDIDO deixa de ter o valor "cancelado" | Registra o cancelamento na granularidade em que ele acontece |
| O horário em que o pedido chega ao cliente não é registrado | Removido `data_hora_entrega` de PEDIDO | Atributo sem fonte de dados |
| A quebra é a divergência de estoque mais comum | Acrescentado `motivo` em MOVIMENTACAO_ESTOQUE | Permite separar quebra, perda, cortesia e consumo interno nos ajustes |
| A entrada nos ambientes é por ordem de chegada, sem registro | Mantido CONTROLE_ACESSO, como proposta de registro de solicitação, entrada e saída | É a solução para o P1; hoje o tempo de espera não é medido |

---

#### Decisões de modelagem

- **VISITA é o eixo do modelo.** Pedidos, pagamentos, comanda e acessos aos ambientes partem dela, e é ela que liga as mesas (N:N, para mesas unidas). O cliente é identificado apenas pela comanda, sem cadastro.
- **FUNCIONARIO é uma única entidade**, com o atributo `cargo`, porque todos os papéis compartilham os mesmos atributos e relacionamentos.
- **ITEM_PEDIDO e ITEM_COMPRA** são entidades próprias, com atributos, no lugar de relacionamentos N:N sem dados, para guardar quantidade e preço praticado de cada item.
- **Os N:N que permanecem** (MESA–VISITA e FUNCIONARIO–PAGAMENTO) viram tabelas associativas no modelo lógico.

---

## 1.2 Conversão do Modelo Conceitual para o Modelo Lógico

O DER de 17 entidades e 22 relacionamentos foi convertido para um modelo relacional com **19 tabelas**: as 17 entidades e 2 tabelas associativas. O DER não tem generalização, atributos multivalorados nem atributos compostos, e por isso a conversão seguiu as regras diretas abaixo. Os tipos de dado e as restrições de cada coluna estão no dicionário de dados.

### Entidades → Tabelas

Cada entidade virou uma tabela com o mesmo nome, em minúsculas e com sublinhado (`controle_acesso`, `item_pedido`, `produto_estoque`). Nenhuma entidade foi fundida com outra nem dividida.

| Entidade | Tabela |
|---|---|
| MESA | `mesa` |
| AMBIENTE | `ambiente` |
| CONTROLE_ACESSO | `controle_acesso` |
| VISITA | `visita` |
| COMANDA | `comanda` |
| PAGAMENTO | `pagamento` |
| FUNCIONARIO | `funcionario` |
| PEDIDO | `pedido` |
| ITEM_PEDIDO | `item_pedido` |
| PRATO | `prato` |
| CATEGORIA_PRATO | `categoria_prato` |
| PRODUTO_ESTOQUE | `produto_estoque` |
| MOVIMENTACAO_ESTOQUE | `movimentacao_estoque` |
| FORNECEDOR | `fornecedor` |
| COMPRA | `compra` |
| ITEM_COMPRA | `item_compra` |
| CAIXA | `caixa` |

### Atributos → Colunas

- Cada atributo virou uma coluna com o mesmo nome do DER.
- Cada identificador (`id_mesa`, `id_visita` e os demais) virou a chave primária da tabela, como inteiro com autoincremento.
- Atributos de data e hora viraram `DATETIME`, valores em reais viraram `DECIMAL(10,2)` e textos viraram `VARCHAR`.
- Atributos que só recebem valor depois de um evento viraram colunas que aceitam nulo: `data_hora_entrada`, `data_hora_saida`, `data_hora_fechamento`, `valor_fechamento`, `data_recebimento`, `numero_nf`, `observacao` e `motivo`.
- Atributos com uma lista fixa de valores (`status`, `forma_pgto`, `tipo`, `cargo`, `setor_producao`) viraram colunas de texto com restrição de valores permitidos.
- Nenhum atributo foi criado, removido ou renomeado na conversão. As colunas de chave estrangeira são as únicas colunas novas.

### Relacionamentos → Chaves

Cada relacionamento 1:N virou uma chave estrangeira na tabela do lado "N". Os N:N viraram tabelas associativas.

| Relacionamento | Entidades | Como foi traduzido |
|---|---|---|
| Possui | AMBIENTE → MESA | `mesa.id_ambiente`, obrigatória |
| Controla | AMBIENTE → CONTROLE_ACESSO | `controle_acesso.id_ambiente`, obrigatória |
| Solicita | VISITA → CONTROLE_ACESSO | `controle_acesso.id_visita`, obrigatória |
| Identifica | VISITA → COMANDA | `comanda.id_visita`, obrigatória e única (1:1) |
| Gera | VISITA → PEDIDO | `pedido.id_visita`, obrigatória |
| Quita | VISITA → PAGAMENTO | `pagamento.id_visita`, obrigatória |
| Atende | FUNCIONARIO → PEDIDO | `pedido.id_func`, obrigatória |
| Contem | PEDIDO → ITEM_PEDIDO | `item_pedido.id_pedido`, obrigatória |
| Refere-se a | PRATO → ITEM_PEDIDO | `item_pedido.id_prato`, obrigatória |
| Classifica | CATEGORIA_PRATO → PRATO | `prato.id_categ`, obrigatória |
| Corresponde a | PRODUTO_ESTOQUE → PRATO | `prato.id_produto`, opcional |
| Executa | FUNCIONARIO → MOVIMENTACAO_ESTOQUE | `movimentacao_estoque.id_func`, obrigatória |
| Movimenta | PRODUTO_ESTOQUE → MOVIMENTACAO_ESTOQUE | `movimentacao_estoque.id_produto`, obrigatória |
| Abastece | COMPRA → MOVIMENTACAO_ESTOQUE | `movimentacao_estoque.id_compra`, opcional |
| Efetua | FUNCIONARIO → COMPRA | `compra.id_func`, obrigatória |
| Fornece | FORNECEDOR → COMPRA | `compra.id_fornecedor`, obrigatória |
| Inclui | COMPRA → ITEM_COMPRA | `item_compra.id_compra`, obrigatória |
| É comprado em | PRODUTO_ESTOQUE → ITEM_COMPRA | `item_compra.id_produto`, obrigatória |
| Opera | FUNCIONARIO → CAIXA | `caixa.id_func`, obrigatória |
| Registra | CAIXA → PAGAMENTO | `pagamento.id_caixa`, obrigatória |
| Ocupa | MESA ↔ VISITA | tabela associativa `mesa_visita` |
| Arrecada | FUNCIONARIO ↔ PAGAMENTO | tabela associativa `funcionario_pagamento` |

### Cardinalidades

- **1:N (19 relacionamentos).** A chave estrangeira fica na tabela que participa com máximo 1. Quando o mínimo desse lado é 1, a coluna é `NOT NULL`. Nos dois relacionamentos com mínimo 0 (`Corresponde a` e `Abastece`), a chave estrangeira aceita nulo: um prato que não é bebida de garrafa ou lata não tem produto de estoque, e uma movimentação de ajuste ou de saída não vem de uma compra.
- **1:1 (`Identifica`).** A chave estrangeira ficou em `comanda` e é `UNIQUE`, de modo que cada visita tem no máximo uma comanda. A comanda foi escolhida como lado da chave porque ela só existe por causa de uma visita. A pulseira volta ao zero depois do 9.999, por isso cada visita cria uma nova linha em `comanda`, e o `codigo` repetido só é permitido em comandas que não estão mais em uso (baixadas ou perdidas).
- **N:N (`Ocupa` e `Arrecada`).** Cada um virou uma tabela associativa. A chave primária é composta pelas duas chaves estrangeiras, o que impede repetir o par:
  - `mesa_visita (id_mesa, id_visita)`: uma visita pode ocupar mais de uma mesa, e uma mesa é ocupada por várias visitas ao longo do tempo.
  - `funcionario_pagamento (id_func, id_pgto)`: o garçom cobra e o caixa finaliza, então um pagamento tem mais de um funcionário.
- **Mínimo 1 no lado N.** O SQL não consegue exigir, na criação das tabelas, que uma visita tenha ao menos uma mesa ou que um pedido tenha ao menos um item. Esses mínimos são verificados por consultas de auditoria no script (visita sem mesa, pedido sem item, compra sem item, pagamento sem funcionário, visita sem pagamento, ambiente sem mesa e categoria sem prato).
- **Ações nas chaves estrangeiras.** Toda chave estrangeira bloqueia a exclusão do registro pai (`RESTRICT`), porque o histórico de visitas, pedidos, pagamentos e estoque não pode ser apagado.

### Generalização e especialização

O DER não tem generalização. Os vários papéis de funcionário (garçom, bar, caixa, maître, gerente e outros) ficaram em uma única tabela `funcionario`, com a coluna `cargo`, porque todos têm os mesmos atributos e os mesmos relacionamentos. Tabelas separadas por cargo duplicariam as colunas e as chaves estrangeiras sem trazer nenhuma coluna própria.



## 1.3 Definição de Chaves Primárias e Estrangeiras

O modelo lógico tem 19 tabelas, 19 chaves primárias e 24 chaves estrangeiras. As colunas marcadas como opcionais aceitam nulo.

| Tabela | Chave primária | Chaves estrangeiras |
|---|---|---|
| `ambiente` | `id_ambiente` | — |
| `mesa` | `id_mesa` | `id_ambiente` → `ambiente` |
| `visita` | `id_visita` | — |
| `comanda` | `id_comanda` | `id_visita` → `visita` (única) |
| `controle_acesso` | `id_controle_acesso` | `id_ambiente` → `ambiente`; `id_visita` → `visita` |
| `funcionario` | `id_func` | — |
| `caixa` | `id_caixa` | `id_func` → `funcionario` |
| `pagamento` | `id_pgto` | `id_visita` → `visita`; `id_caixa` → `caixa` |
| `pedido` | `id_pedido` | `id_visita` → `visita`; `id_func` → `funcionario` |
| `item_pedido` | `id_item` | `id_pedido` → `pedido`; `id_prato` → `prato` |
| `categoria_prato` | `id_categ` | — |
| `prato` | `id_prato` | `id_categ` → `categoria_prato`; `id_produto` → `produto_estoque` (opcional) |
| `produto_estoque` | `id_produto` | — |
| `fornecedor` | `id_fornecedor` | — |
| `compra` | `id_compra` | `id_func` → `funcionario`; `id_fornecedor` → `fornecedor` |
| `item_compra` | `id_item_compra` | `id_compra` → `compra`; `id_produto` → `produto_estoque` |
| `movimentacao_estoque` | `id_movimentacao` | `id_func` → `funcionario`; `id_produto` → `produto_estoque`; `id_compra` → `compra` (opcional) |
| `mesa_visita` | (`id_mesa`, `id_visita`) | `id_mesa` → `mesa`; `id_visita` → `visita` |
| `funcionario_pagamento` | (`id_func`, `id_pgto`) | `id_func` → `funcionario`; `id_pgto` → `pagamento` |


## 1.4 Processo de Normalização (1FN, 2FN e 3FN)

O DER já foi desenhado com entidades separadas, e a normalização foi usada para conferir cada tabela do modelo lógico e para decidir os pontos duvidosos. O resultado final está na 3FN.

### 1FN: sem grupos repetitivos nem atributos multivalorados

Uma tabela está na 1FN quando cada coluna guarda um único valor e não há grupos que se repetem. No Ponto A, os casos que poderiam ferir a 1FN eram:

| Situação | Como ficaria fora da 1FN | Como ficou |
|---|---|---|
| Um pedido tem vários itens | colunas `item1`, `item2`, `item3` ou um campo com a lista de pratos | `item_pedido`, uma linha por item |
| Uma compra tem vários itens | uma lista de produtos na compra | `item_compra`, uma linha por item |
| Uma visita ocupa mais de uma mesa | um campo "mesas" com vários números | `mesa_visita`, uma linha por mesa da visita |
| Um pagamento tem mais de um funcionário (garçom e caixa) | colunas `funcionario1` e `funcionario2` | `funcionario_pagamento`, uma linha por funcionário |
| Uma visita pode ter vários pagamentos (divisão de conta) | vários valores no mesmo registro | `pagamento`, uma linha por pagamento |

As demais colunas guardam valores atômicos. O nome do funcionário fica em uma coluna só (`nome_func`), porque o sistema nunca separa nome e sobrenome. Data e hora ficam juntas em uma coluna `DATETIME`, e a observação do item é um texto livre, sem lista.

### 2FN: sem dependências parciais

A 2FN exige que, em tabelas com chave composta, cada coluna dependa da chave inteira. Só duas tabelas têm chave composta, `mesa_visita` e `funcionario_pagamento`, e nenhuma delas tem outras colunas além da chave. Nas outras 17 tabelas a chave primária é uma coluna única, e uma dependência parcial é impossível.

O ponto que parece uma dependência parcial e não é: `item_pedido.preco_unitario` poderia ser lido como "o preço do prato". Ele depende do item, porque guarda o preço praticado naquele momento, que pode ser diferente do preço atual em `prato.preco`. O mesmo vale para `item_compra.custo_unitario`. Os dois são fatos históricos e ficam nas tabelas dos itens.

### 3FN: sem dependências transitivas

A 3FN exige que cada coluna dependa só da chave, e não de outra coluna que não seja chave. Os dados de outra entidade ficam na tabela dela, e a tabela de origem guarda só a chave estrangeira:

| Tabela | Não guarda | Porque depende de | Está em |
|---|---|---|---|
| `prato` | nome da categoria e `setor_producao` | da categoria, não do prato | `categoria_prato` |
| `mesa` | nome e capacidade do ambiente | do ambiente, não da mesa | `ambiente` |
| `pedido` | nome e cargo do funcionário | do funcionário, não do pedido | `funcionario` |
| `item_pedido` | nome do prato | do prato, não do item | `prato` |
| `compra` | razão social e CNPJ do fornecedor | do fornecedor, não da compra | `fornecedor` |
| `movimentacao_estoque` e `item_compra` | nome do produto | do produto, não da movimentação | `produto_estoque` |
| `controle_acesso` | capacidade do ambiente | do ambiente | `ambiente` |

Duas colunas guardam um valor que dá para calcular a partir de outras linhas: `produto_estoque.qtd_atual` (soma das movimentações) e `compra.valor_total` (soma dos itens). Elas não dependem de outra coluna da mesma linha, então não ferem a 3FN, mas são redundância controlada. `qtd_atual` evita somar milhares de movimentações a cada consulta, e `valor_total` confere com a nota do fornecedor. Consultas de auditoria comparam cada valor com a soma dos registros de origem. O `pagamento.valor_total` não entra aqui: é o valor efetivamente pago, que muda com a divisão da conta, e por isso é um dado do pagamento e não um cálculo.

### Justificativa do processo

- **1FN:** a casa registra de 2.000 a 5.000 pedidos por dia, e cada pedido tem um número variável de itens. Sem itens em linhas separadas, não seria possível calcular o cancelamento por item, o consumo por mesa nem a diferença entre saídas de estoque e vendas.
- **2FN:** as duas tabelas associativas guardam só a ligação entre os registros. Sem outras colunas, não há como surgir uma dependência parcial, e o preço do momento fica no item.
- **3FN:** o cardápio, as categorias e os funcionários mudam com frequência. Cada dado fica em um só lugar, então trocar o setor de uma categoria, o cargo de um funcionário ou o telefone de um fornecedor é uma única alteração, sem risco de dados diferentes para o mesmo fato.

## 1.5 Justificativas Técnicas das Decisões

### Chaves primárias

Todas as tabelas de entidade usam uma chave primária própria, inteira e gerada pelo sistema (`id_*`). Nenhum dado do negócio serve como chave:

- `comanda.codigo` volta a zero depois do 9.999, então se repete ao longo do tempo (RN02).
- `mesa.numero` e `ambiente.nome_ambiente` são rótulos que a casa pode mudar.
- `pagamento.numero_nf` só existe depois da emissão da nota (RN16).
- O banco não guarda CPF de funcionário nem de cliente (RN27), e por isso não há chave natural para `funcionario`.
- Chaves inteiras são menores e mais rápidas nas tabelas grandes (`pedido` e `item_pedido`), e nas chaves estrangeiras que as referenciam.

As duas tabelas associativas usam chave composta pelas duas chaves estrangeiras. Isso impede repetir o mesmo par e dispensa uma coluna a mais.

### Chaves estrangeiras

| Chave estrangeira | Relacionamento que implementa | Por quê |
|---|---|---|
| `mesa.id_ambiente` | Possui | cada mesa pertence a um único ambiente (RN05) |
| `controle_acesso.id_ambiente`, `controle_acesso.id_visita` | Controla, Solicita | cada acesso é de uma visita e de um ambiente, para medir fila e ocupação (RN06, RN07) |
| `comanda.id_visita` (única) | Identifica | uma visita tem uma pulseira, e uma pulseira em uso identifica uma só visita (RN01) |
| `pedido.id_visita`, `pedido.id_func` | Gera, Atende | todo consumo pertence a uma visita e a um funcionário responsável (RN04, RN09, RN29) |
| `item_pedido.id_pedido`, `item_pedido.id_prato` | Contem, Refere-se a | liga cada item ao pedido e ao prato, com o preço do momento (RN10) |
| `prato.id_categ` | Classifica | cada prato pertence a uma única categoria, que define o setor de produção (RN11, RN12) |
| `prato.id_produto` (opcional) | Corresponde a | só a bebida vendida como vem (garrafa ou lata) baixa o estoque (RN23) |
| `pagamento.id_visita`, `pagamento.id_caixa` | Quita, Registra | o pagamento quita uma visita e é finalizado em um caixa (RN17, RN18) |
| `caixa.id_func` | Opera | cada turno de caixa tem um funcionário responsável (RN19, RN29) |
| `movimentacao_estoque.id_func`, `id_produto` | Executa, Movimenta | toda movimentação identifica quem a fez e o produto (RN20, RN21, RN29) |
| `movimentacao_estoque.id_compra` (opcional) | Abastece | a entrada vinda de uma compra fica rastreável (RN25) |
| `compra.id_func`, `compra.id_fornecedor` | Efetua, Fornece | a compra é de um fornecedor cadastrado e feita por um funcionário (RN24, RN29) |
| `item_compra.id_compra`, `item_compra.id_produto` | Inclui, É comprado em | liga cada item da compra ao produto, com o custo do momento (RN24) |
| `mesa_visita` (duas chaves) | Ocupa | uma visita ocupa uma ou mais mesas (RN05) |
| `funcionario_pagamento` (duas chaves) | Arrecada | o garçom cobra e o caixa finaliza (RN18, RN29) |

Todas as chaves estrangeiras bloqueiam a exclusão do registro pai (`RESTRICT`), porque o histórico de visitas, pedidos, pagamentos e estoque não pode ser apagado (RN13, RN21).

### Restrições (unicidade, obrigatoriedade e valores permitidos)

| Tabela e coluna | Restrição | Regra | Por quê |
|---|---|---|---|
| `comanda.codigo` | de 0 a 9.999; único entre as comandas em uso | RN02 | a pulseira se repete depois do 9.999, mas duas comandas em uso nunca têm o mesmo número |
| `comanda.status` | em uso, baixada ou perdida; padrão em uso | RN03, RN04 | controla a pulseira da entrega à baixa |
| `visita.status`, `data_hora_fechamento` | aberta ou fechada; fechada exige data de fechamento, nunca anterior à abertura | RN04 | a visita só fecha com a conta quitada |
| `controle_acesso.quant_pessoas` | maior que zero | RN06, RN07 | entra no cálculo da ocupação |
| `controle_acesso.data_hora_entrada`, `data_hora_saida` | entrada não anterior à solicitação; saída só com entrada e não anterior a ela | RN06 | a ocupação corrente só é calculável se toda entrada e saída forem registradas |
| `controle_acesso.status` | aguardando, no ambiente, encerrado ou desistiu | RN06 | acompanha a fila |
| `ambiente.capacidade_maxima` | maior que zero | RN07 | base do cálculo de ocupação |
| `ambiente.nome_ambiente`, `mesa.numero` | únicos e obrigatórios | RN05 | identificam o ambiente e a mesa, sem repetição |
| `pedido.status` | registrado, em preparo, pronto ou entregue | RN09 | acompanha o pedido |
| `item_pedido.qtd`, `preco_unitario` | maiores que zero | RN10 | o item guarda a quantidade e o preço do momento |
| `item_pedido.status` | ativo ou cancelado; padrão ativo | RN13 | o cancelamento é por item, e o item permanece |
| `prato.preco` | maior que zero | RN10 | preço vigente do cardápio |
| `prato.id_categ` | obrigatória | RN12 | cada prato tem uma única categoria |
| `categoria_prato.setor_producao`, `nome` | setor entre bar, cozinha, narguilé, comida japonesa e pizzaria; nome único | RN11 | define para onde o item segue |
| `pagamento.valor_total` | maior que zero | RN17 | cada pagamento quita uma parte da conta |
| `pagamento.valor_taxa_servico` | de zero até `valor_total`; padrão zero | RN15 | a taxa é parte do pagamento |
| `pagamento.forma_pgto` | dinheiro, cartão de crédito, cartão de débito, pix ou ticket | RN17 | formas aceitas pela casa |
| `pagamento.numero_nf` | único quando preenchido | RN16 | duas contas não dividem a mesma nota; fica vazio até a emissão |
| `caixa.valor_abertura` | maior ou igual a zero; padrão 150,00 | RN19 | fundo de troco do Ponto A |
| `caixa.data_hora_fechamento`, `valor_fechamento` | obrigatórios quando o caixa está fechado | RN19 | o fechamento é conferido pelo maître ou pela gerência |
| `produto_estoque.qtd_atual`, `estoque_minimo` | maiores ou iguais a zero | RN22 | o estoque nunca fica negativo |
| `produto_estoque.nome` | único e obrigatório | RN22 | um produto por linha |
| `movimentacao_estoque.tipo`, `qtd` | entrada, saída, ajuste positivo ou ajuste negativo; quantidade maior que zero | RN20, RN21 | o sentido da movimentação é dado pelo tipo |
| `movimentacao_estoque.motivo` | obrigatório quando o tipo é ajuste | RN21 | explica a divergência (quebra, perda, cortesia) |
| `fornecedor.cnpj` | único e obrigatório; `telefone` obrigatório | RN24 | a compra é só de fornecedor cadastrado |
| `compra.valor_total`, `status` | maior que zero; pedida, recebida ou cancelada; padrão pedida | RN24, RN25 | a compra cancelada permanece registrada |
| `compra.data_recebimento` | obrigatória quando recebida | RN25 | registra quando os maîtres conferem a entrega |
| `item_compra.qtd`, `custo_unitario` | maiores que zero | RN24 | guarda o custo do momento |
| `funcionario.nome_func`, `cargo` | obrigatórios; cargo entre os 14 do Ponto A | RN28, RN29 | o nível de acesso depende do cargo |

Algumas regras não viram restrição no banco e ficam no controle de acesso, no processo ou em consultas de auditoria:

- **RN07:** o limite de capacidade do ambiente é verificado pela consulta de ocupação, porque depende da soma das visitas presentes.
- **RN13, RN14, RN28:** quem cancela, estorna ou altera conta fechada é definido pelos privilégios por cargo, e não por uma coluna.
- **RN21:** a imutabilidade das movimentações de estoque é garantida retirando os privilégios de alteração e exclusão dessa tabela.
- **RN08 e RN26:** não geram dado. A venda de bebida e comida em todos os ambientes e a conferência da idade são do processo.

### Decisões arquiteturais gerais

- **Histórico preservado.** Nada é apagado: cancelamentos, estornos e compras canceladas viram `status`, e as chaves estrangeiras usam `RESTRICT`. Isso sustenta os indicadores de cancelamento, de diferença de caixa e de estoque.
- **Preço do momento.** O preço e o custo ficam no item (`preco_unitario` e `custo_unitario`), para o faturamento e o gasto continuarem corretos quando o cardápio ou o fornecedor mudarem.
- **Valores permitidos como texto com restrição.** Status, tipo, forma de pagamento e cargo são `VARCHAR` com `CHECK`, e não `ENUM`, para mudar a lista de valores sem alterar a estrutura da tabela.
- **Mínimo 1 do lado N.** O SQL não impõe esse mínimo (por exemplo, visita com ao menos uma mesa). Consultas de auditoria no script verificam esses casos.
- **Colunas calculáveis mantidas.** `qtd_atual` e `compra.valor_total` ficam gravados, por desempenho e por conferência com a nota do fornecedor, com auditoria comparando-os com a soma dos registros.
- **Sem cadastro de clientes.** O modelo não tem tabela de clientes: a pulseira identifica a visita, e nenhum dado pessoal de cliente é guardado (RN27).
- **Uma tabela de funcionários.** O cargo é uma coluna de `funcionario`, porque todos os cargos têm os mesmos atributos e relacionamentos.
- **Tecnologia.** MySQL 8, motor InnoDB, conjunto de caracteres `utf8mb4`. O InnoDB dá suporte às chaves estrangeiras e às transações que o registro de pagamentos e de estoque exige, e o MySQL 8 aceita `CHECK` e colunas geradas.


## 1.6 Proposta de Arquitetura Analítica para BI e IA

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



## 2. Modelo Lógico


O modelo lógico foi gerado no brModelo a partir do DER conceitual revisado (seção 1.2) e tem **19 tabelas**: as 17 entidades do DER mais duas tabelas associativas criadas na conversão dos relacionamentos muitos-para-muitos. [Modelo Lógico](modelo_logico.jpg)

- `mesa_visita`: liga a visita às mesas que ela ocupa, já que uma visita pode usar mais de uma mesa. A chave primária é composta (`id_mesa`, `id_visita`).
- `funcionario_pagamento`: liga o pagamento aos funcionários que o receberam, já que o garçom ou o caixa pode receber e a conta pode ser dividida. A chave primária é composta (`id_pgto`, `id_func`).

No total são **19 chaves primárias** e **24 chaves estrangeiras**. Os pontos que merecem atenção na leitura do modelo são:

- **Visita e comanda (1:1).** A chave estrangeira fica em `comanda.id_visita`, com restrição `UNIQUE`, para que cada visita tenha uma única pulseira.
- **Chaves estrangeiras opcionais.** `prato.id_produto` pode ficar vazia, porque drinks e doses não baixam o estoque, e só a bebida vendida como vem se liga a um produto. `movimentacao_estoque.id_compra` só é preenchida nas entradas de estoque que vêm de uma compra.
- **Preço do momento.** O valor cobrado fica em `item_pedido.preco_unitario`, e não só em `prato.preco`, para que mudanças no cardápio não alterem as contas já feitas.
- **Cardinalidades.** O brModelo mostra cada ligação do lado da tabela de origem como (1,1) e do lado da tabela dependente como (0,n), e (0,1) nas ligações 1:1. Isso é só a convenção da ferramenta e não muda as regras do DER.

Os tipos de dados do brModelo são limitados, então o modelo mostra só a estrutura (tabelas, chaves e ligações). Os tipos finais (`INT`, `VARCHAR(n)`, `DECIMAL(10,2)`, `DATETIME`), as restrições `CHECK` e as regras de exclusão (`RESTRICT` em todas as chaves estrangeiras) estão definidos no script da seção 3 e no dicionário de dados.


## 3. Script SQL

O script está em [`docs/script.sql`](docs/script.sql) e roda do zero em um banco vazio (começa com `DROP DATABASE IF EXISTS` e `CREATE DATABASE ponto_a`). Ele segue esta ordem:

1. **CREATE DATABASE** (`utf8mb4`).
2. **CREATE TABLE** das 19 tabelas, com chaves primárias, chaves estrangeiras (todas `RESTRICT`), `UNIQUE`, `NOT NULL` e `CHECK`. Cada restrição tem nome (`pk_`, `fk_`, `uq_`, `ck_`).
3. **INSERT** da massa de dados (a mesma de `docs/massa_de_dados.sql`).
4. **SELECT com JOIN** (S01–S05).
5. **UPDATE** (U01–U03) e **DELETE** (D01), mais exemplos comentados de operações que o banco recusa.
6. **Consultas gerenciais** G01–G16, uma para cada indicador K1–K16 da seção 1.6. Cada uma traz no cabeçalho a pergunta e o `Atende:`.
7. **Consultas de auditoria** A01–A22. Cada uma traz o `Verifica:` com a regra de negócio. Uma auditoria que retorna zero linhas indica que a regra está sendo cumprida.

Decisões que não estão no modelo conceitual:
- `VARCHAR` com `CHECK` no lugar de `ENUM`, para os valores aceitos ficarem visíveis na própria tabela.
- A unicidade do número da pulseira entre as comandas em uso é feita com uma coluna gerada (`codigo_em_uso`) com `UNIQUE`.
- As FKs opcionais são `prato.id_produto` (drinks e doses não ligam ao estoque) e `movimentacao_estoque.id_compra` (só as entradas ligam a uma compra).

Auditorias que retornam linhas de propósito: A16 (2 pagamentos sem nota fiscal), A19 (5 pratos vendidos por preço diferente do atual, o que mostra o preço do momento guardado no item) e A22 (3 pulseiras perdidas localizadas por garçom e mesa).



## 4. Massa de dados

Arquivo: [`docs/massa_de_dados.sql`](docs/massa_de_dados.sql). Os dados são fictícios e cobrem a semana de 26/10/2026 (segunda) a 01/11/2026 (domingo), com as compras da semana anterior para formar o estoque.

| Tabela | Linhas | Tabela | Linhas |
|---|---|---|---|
| ambiente | 3 | compra | 82 |
| mesa | 55 | item_compra | 102 |
| funcionario | 67 | movimentacao_estoque | 458 |
| categoria_prato | 5 | visita / comanda | 183 / 183 |
| prato | 400 | controle_acesso | 190 |
| produto_estoque | 60 | pedido | 1.354 |
| fornecedor | 50 | item_pedido | 2.427 |
| caixa | 14 | pagamento | 240 |
| mesa_visita | 236 | funcionario_pagamento | 323 |

**Por que esse volume.** O Ponto A tem 55 mesas, 67 funcionários, 400 a 800 itens de cardápio e 2.000 a 5.000 pedidos por dia. O cadastro (mesas, funcionários, produtos, fornecedores) está em escala real. O movimento é uma amostra: cerca de um décimo do volume diário em uma semana, o suficiente para os indicadores terem o que mostrar sem tornar o script pesado para o professor rodar.

**Variedade incluída de propósito:**
- itens cancelados (93) e pedidos em todos os status;
- produtos no estoque mínimo ou abaixo (8) e dois em ruptura;
- as cinco formas de pagamento e contas divididas em mais de um pagamento;
- fila com desistências (Deck, nos dias de maior movimento) e acessos ainda no ambiente;
- ajustes de estoque (quebra, perda, sobra, cortesia);
- pulseiras perdidas, compras recebidas, pedidas e canceladas;
- diferenças de caixa no fechamento e pagamentos sem nota fiscal.

Obs.: o DELETE D01 remove um dos fornecedores sem compra, por isso o banco termina com 49 fornecedores depois de rodar o script inteiro.

**Como testar:** `mysql -u root -p < docs/script.sql` em um banco vazio.



## 5\. Repositório GitHub

- Relatório técnico: este `README.md`.
- [Dicionário de dados](dicionario_dados.html)
- [DER revisado](DER.jpg) e [modelo lógico](modelo_logico.jpg)
- [Levantamento](levantamento/levantamento.md) e [evidência 1](evidencias/fotoEntrevista.jpg) [evidência 2](evidencias/fotoEntrevista2.jpg) [evidência 3](fotoFachada.jpg)
- [Script SQL](docs/script.sql) e [massa de dados](docs/massa_de_dados.sql)

---

## Seção 6 — Uso de Inteligência Artificial

> Rascunho acumulado durante a Entrega 2.
> Ferramenta em todas as entradas: **Claude (Anthropic), modelo Claude Sonnet 5.5**, via Claude Code / claude.ai.
> Integrantes (conforme correção da Entrega 1): Diego Xavier Ribeiro, Luiz Felipe de Brito Carvalho, Nicolas Vieira de Lima, Renan Caio de Lima.

---

### Uso 01 — Análise do esqueleto da Entrega 2
- **Ferramenta e etapa:** Claude — leitura e resumo do esqueleto `00-d_Esqueleto_Entrega_2` (planejamento da entrega).
- **Motivação:** entender de uma vez todas as exigências, pesos e armadilhas do arquivo antes de produzir qualquer artefato.
- **Prompt:** "Estamos agora na entrega 2, analise todo o arquivo primeiro" (anexo: esqueleto da Entrega 2 em .odt).
- **Resposta recebida:** resumo por seção (README, 1.1 a 1.6, modelo lógico, script SQL, massa de dados, repositório, seção 6, apresentação, pesos), com pontos de atenção para o projeto (escopo do DER, evidências da organização real, requisitos, entidade associativa de itens, notação brModelo).
- **Fontes consultadas e verificadas:** apenas o próprio esqueleto fornecido pelo professor.
- **Trechos rejeitados ou corrigidos:** nenhum 
- **Justificativa da escolha final:** usado como checklist de trabalho.
- **Reflexão crítica:** resumo de IA pode omitir detalhes; o esqueleto original continua sendo a fonte oficial. A IA cometeu um erro de redação no resumo ("Brasil/Ponto A é real"), apontando falha de revisão que precisou ser ignorada.

### Uso 02 — Mapeamento da correção da Entrega 1 (Caso A, seção 1.1)
- **Ferramenta e etapa:** Claude — transformar a correção recebida em tabela de apontamentos → ações.
- **Motivação:** o esqueleto trata apontamento não citado na tabela como não corrigido.
- **Prompt:** colagem do texto integral da correção da Entrega 1 (nota 1,100/1,60) com o comentário "colei a correção"; depois: "os integrantes são os mesmos no texto colado e na correção".
- **Resposta recebida:** tabela de apontamentos (ITEM_PEDIDO, nome no PRATO, AMBIENTE/ACESSO, ESTOQUE, CAIXA, FORNECEDOR/COMPRA, requisitos de outro sistema, regras e processos faltantes, dicionário, entrevista com responsável) e alertas (integrantes, declaração de uso de IA, números de porte).
- **Fontes consultadas e verificadas:** texto da correção do professor. 
- **Trechos rejeitados ou corrigidos:** nenhum
- **Justificativa da escolha final:** usado para guiar os integrantes
- **Reflexão crítica:** a IA assumiu inicialmente uma lista de integrantes diferente da correção; o grupo corrigiu. Registro importante: na Entrega 1 o grupo declarou não ter usado IA; nesta entrega o uso é declarado.

### Uso 03 — Revisão do DER ampliado (BRModelo Web)
- **Ferramenta e etapa:** Claude — conferência do DER revisado enviado em PDF (`Personal - Conceptual model - BRMW.pdf`) contra a correção e o esqueleto.
- **Motivação:** verificar se o DER cobre os problemas P1 (acesso ao Deck) e P2 (estoque de bebidas) antes de converter para o modelo lógico.
- **Prompt:** envio do PDF do DER com o texto "Mandei o arquivo do DER com algumas coisas a mais que coloquei".
- **Resposta recebida:** DER lido com 14 entidades (inclui AMBIENTE, CONTROLE_ACESSO, ITEM_PEDIDO, MOVIMENTACAO_ESTOQUE, PRODUTO_ESTOQUE). Pontos levantados: cardinalidades (1,n) nos dois lados de CONTROLE_ACESSO, ITEM_PEDIDO e MOVIMENTACAO_ESTOQUE (o lado "filho" deveria ser (1,1)); MOVIMENTACAO_ESTOQUE e PRODUTO_ESTOQUE sem atributos; FUNCIONARIO sem nome; faltam datas/horas em VISITA e PEDIDO e forma de pagamento em PAGAMENTO (necessários para KPIs e para a massa de dados); sem ligação entre PRATO (bebida) e PRODUTO_ESTOQUE.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

---
*Próximos usos previstos:* modelo lógico, normalização, script SQL, massa de dados (inclui prompts e ajustes manuais), consultas G/A, dicionário HTML, README e slides.

### Uso 04 — Ajuste das cardinalidades e inclusão de FORNECEDOR, COMPRA, CAIXA
- **Ferramenta e etapa:** Claude — definição das cardinalidades corrigidas e dos novos relacionamentos do DER revisado.
- **Motivação:** aplicar os apontamentos da correção (itens ausentes: fornecedor/compra e caixa; estoque sem ligação com o cardápio).
- **Prompt:** "me ajude a aplicar o item 1 (dizendo o que tem de cada lado), 5 (vamos criar esse relacionamento, e vamos criar o fornecedor e o caixa)".
- **Resposta recebida:** tabela com (mín,máx) dos dois lados de seis relacionamentos corrigidos; relacionamento PRATO (0,1) – PRODUTO_ESTOQUE (0,n); novas entidades FORNECEDOR, COMPRA, ITEM_COMPRA, CAIXA e SANGRIA com atributos e cardinalidades; renomeação de quatro relacionamentos "registra".
- **Fontes consultadas e verificadas:** correção da Entrega 1; processos do Ponto A (caixa com abertura, sangria e fechamento).
- **Trechos rejeitados ou corrigidos:** nenhum
- **Justificativa da escolha final:** primeira parte do projeto "feita" (mudou depois da entrevista).
- **Reflexão crítica:** a IA propôs "1 prato = 1 unidade do produto" sem fator de conversão (simplificação); a necessidade de ITEM_COMPRA e SANGRIA como entidades foi decisão de modelagem a validar.

### Uso 05 — Conferência do DER ampliado (19 entidades)
- **Ferramenta e etapa:** Claude — revisão do DER redesenhado no BRModelo Web (PDF `Personal - Conceptual model - BRMW.pdf`, 2ª versão) contra a correção da Entrega 1 e o esqueleto da Entrega 2.
- **Motivação:** confirmar que o DER cobre os problemas P1 e P2 e os apontamentos do professor antes da conversão para o modelo lógico.
- **Prompt:** "agora cobre tudo que precisamos?" (anexo: PDF do DER atualizado).
- **Resposta recebida:** confirmação das cardinalidades corrigidas e dos novos relacionamentos; lacunas: PAGAMENTO sem `forma_pagamento`/`data_hora` (e sem taxa de serviço e cupom fiscal), COMANDA sem `status`, losango `Rel` sem nome, `recebe` repetido, PEDIDO sem `data_hora_entrega`, ITEM_PEDIDO sem `observacao`, CATEGORIA_PRATO sem setor de produção, mínimos (1,n) que impedem cliente sem visita; processos a justificar (reservas, escala, normas sanitárias, horário).
- **Fontes consultadas e verificadas:** correção do professor, esqueleto da Entrega 2 e DER enviado.
- **Trechos rejeitados ou corrigidos:** nenhum
- **Justificativa da escolha final:** ...
- **Reflexão crítica:** a IA leu o DER a partir de imagem rasterizada do PDF; pequenos rótulos podem ter sido mal lidos e precisam ser conferidos no BRModelo.

### Uso 06 — Conferência final do DER e decisão sobre processos não incluídos
- **Ferramenta e etapa:** Claude — conferência do DER após as alterações do grupo (PDF `Personal - Conceptual model - BRMW.pdf`, 3ª versão) e redação de justificativa para processos omitidos.
- **Motivação:** garantir que o DER revisado corresponde à correção antes de converter para o modelo lógico.
- **Prompt:** "analise agora. As reservas, escala, normas, horarios ficam de fora por serem informações que ao meu ver não caberiam num banco de dados focado no processo interno do restaurante, a reserva por exemplo funcionarios podem fazer e seria tratado num outro momento, todo o resto são coisas individuais".
- **Resposta recebida:** confirmação de que PAGAMENTO, COMANDA, ITEM_PEDIDO e CATEGORIA_PRATO receberam os atributos sugeridos e os losangos foram renomeados; pendências: erros de digitação (`observances`, `setor_produto`), padronização de nomes (`qtd`/`quantidade`, `id_movis`), mínimos (1,n) em Cliente e Mesa, ausência de `data_hora_entrega`; rascunho de justificativa no README para reservas, escala, normas sanitárias e horário.
- **Decisão do grupo (não da IA):** reservas, escala da equipe, normas sanitárias e horário de funcionamento ficam fora do modelo, por não pertencerem ao processo interno de atendimento, consumo, caixa e estoque; reservas serão tratadas em outro momento.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 07 — Conversão do DER para o modelo lógico
- **Ferramenta e etapa:** Claude — conversão do DER revisado (19 entidades) em modelo lógico relacional: 23 tabelas, PKs/FKs, tipos, restrições; diagrama gerado com Graphviz a partir de um esquema em Python.
- **Motivação:** produzir o modelo lógico (seções 1.2 e 1.3 e diagrama) a partir do DER aprovado pelo grupo.
- **Prompt:** envio do DER final com o texto "acho que agora esta tudo certo" (continuação do pedido anterior de iniciar o modelo lógico).
- **Resposta recebida:** `modelo_logico.md` (conversão, PKs/FKs, restrições por coluna), `modelo_logico.png/.svg` (diagrama com (mín,máx) nas pontas). Decisões: 4 tabelas associativas para os N:N; `visita.id_comanda` UNIQUE NOT NULL para o 1:1; FKs opcionais em `prato.id_produto` e `movimentacao_estoque.id_compra`.
- **Fontes consultadas e verificadas:** DER do grupo (BRModelo Web).
- **Trechos rejeitados ou corrigidos:** erros de digitação do DER (`cps_cliente`, `std`) interpretados como `cpf_cliente` e `quantidade`. **[GRUPO]** outros ajustes.
- **Justificativa da escolha final:** ...
- **Reflexão crítica:** o diagrama foi gerado com Graphviz (não é o brModelo); o esqueleto pede a notação visual do brModelo, então o grupo precisa refazer o desenho nele (ou aceitar o risco). A IA ajustou rótulos que se sobrepunham. Tipos e tamanhos de coluna são sugestões a validar com os dados reais.

### Uso 07 — Confirmação final do DER (19 entidades)
- **Ferramenta e etapa:** Claude — última conferência do DER no BRModelo Web (4ª versão) antes do modelo lógico.
- **Prompt:** "acho que agora esta tudo certo, apenas confirme isso por enquanto n precisa gerar nd" (anexo: PDF do DER).
- **Resposta recebida:** DER cobre P1, P2 e os apontamentos; mínimos de Cliente e Mesa já em (0,n); `data_hora_entrega` adicionada; restaram dois erros de digitação de atributos (`cps_cliente` → `cpf_cliente`; `std` em ITEM_PEDIDO → `quantidade`/`qtd`).
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 08 — Comparação do README da Entrega 1 com as exigências da Entrega 2
- **Ferramenta e etapa:** Claude — levantamento do que já está pronto, o que precisa de ajuste e o que falta.
- **Motivação:** planejar o trabalho restante sem refazer o que já atende ao esqueleto.
- **Prompt:** "agora analise a entrega 1 que esta acima e compare com a entrega 2 para saber oq ja esta feito" (colagem do README da Entrega 1).
- **Resposta recebida:** tabela por seção do esqueleto com status (pronto, parcial, refazer, falta). Resultado: DER, evidências e justificativa prontos; requisitos e dicionário a refazer; seções 1.2 a 1.6, modelo lógico, SQL, massa de dados, slides e entrevista ainda inexistentes; pendências do grupo (usuários do GitHub, contato do responsável, entrevista, números de volume).
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 09 — Redação da seção do DER para o README (inclui tabela do Caso A)
- **Ferramenta e etapa:** Claude — redação em Markdown da seção "Diagrama Entidade-Relacionamento (DER)" do README, a partir do DER final enviado pelo grupo.
- **Motivação:** substituir a seção do DER da Entrega 1 (9 entidades) pela versão revisada (19 entidades), com a tabela de apontamentos do Caso A.
- **Prompt:** "vamos já adicionar o DER na seção dele, preciso que me entregue em .md para ja substituir a seção".
- **Resposta recebida:** arquivo `secao_der.md` com software utilizado (BRModelo Web), tabela de entidades e atributos, tabela de 24 relacionamentos com cardinalidades nos dois lados, tabela do Caso A (apontamento, o que foi feito, por quê) e decisões de modelagem.
- **Fontes consultadas e verificadas:** DER do grupo (PDF) e correção da Entrega 1.
- **Trechos rejeitados ou corrigidos / justificativa / reflexão:** nenhum(a)

### Uso 10 — Alinhamento com o esqueleto da Entrega 2 em Markdown
- **Ferramenta e etapa:** Claude — conferência do esqueleto em .md contra a análise feita a partir do .odt e ajuste do nível de título da seção do DER.
- **Prompt:** "Aqui a entrega 2 num arquivo md também, assim falamos da mesma coisa" (anexo: esqueleto da Entrega 2 em .md).
- **Resposta recebida:** confirmação de que o conteúdo é o mesmo do .odt; ajuste de `secao_der.md` para ficar dentro da seção 1.1 (título "DER conceitual revisado (Caso A)").
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 11 — Roteiro de entrevista para o responsável do Ponto A
- **Ferramenta e etapa:** Claude — elaboração do roteiro de entrevista em Word (levantamento de requisitos, processos, regras e volumes).
- **Motivação:** o esqueleto exige entrevista com o responsável no local, com números de porte, observação presencial e confirmação de processos, requisitos e regras.
- **Prompt:** "um colega do grupo fará a entrevista, preciso de perguntas que sejam pertinentes e corroborem para o projeto e as decisões, exporte num word com áreas para ele colocar a resposta do entrevistado. campos para nome do entrevistado e entrevistador, cargo do entrevistado."
- **Resposta recebida:** `roteiro_entrevista_ponto_a.docx` com campos de identificação, 39 perguntas em 8 blocos (porte, atendimento, acesso aos ambientes, estoque, compras, caixa, regras, KPIs), cada uma com a indicação de onde a resposta entra no projeto e área de resposta; seção de observação presencial e encerramento (autorização do nome e telefone, foto).
- **Fontes consultadas e verificadas:** correção da Entrega 1, esqueleto da Entrega 2 e DER final.
- **Trechos rejeitados ou corrigidos / justificativa / reflexão:** nenhum(a)

### Uso 12 — Atualização do dicionário de dados (de 9 para 19 entidades)
- **Ferramenta e etapa:** Claude — extensão do `dicionario_dados.html` da Entrega 1 para as 19 entidades do DER revisado, com restrições de integridade por coluna.
- **Motivação:** o esqueleto exige dicionário atualizado, no modelo do exemplo, com obrigatoriedade, unicidade, valores permitidos e valor padrão de cada coluna.
- **Prompt:** "já vamos adiantar o dicionário, preciso que você apenas adicione as outras entidades, o estilo continua o mesmo" (anexo: dicionario_dados.html da Entrega 1).
- **Resposta recebida:** HTML v2.0 com as 10 entidades novas, atributos novos nas já existentes, relacionamentos (24), fluxo de dados, matriz de acesso e histórico de revisões. Decisões da IA a validar pelo grupo: domínios fechados de ambiente (ativo/inativo), acesso (aguardando/no ambiente/encerrado/desistiu), comanda (em uso/baixada/perdida), forma de pagamento (dinheiro/crédito/débito/pix), movimentação (entrada/saída/ajuste positivo/ajuste negativo) e setor de produção.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 13 — Análise do documento de regras não incluídas e KPIs feito por um integrante
- **Ferramenta e etapa:** Claude — revisão de um rascunho de um integrante (regras de negócio não incluídas e arquitetura analítica para BI e IA) contra o DER final e o esqueleto da Entrega 2.
- **Motivação:** descobrir contradições com o DER e o que falta para atender a seção 1.1 e a seção 1.6.
- **Prompt:** "Documento que um dos integrantes fez, não está 100% ele disse, é só para dar uma analisada e ver o que falta" (anexo: documento1.odt).
- **Resposta recebida:** lista de contradições (compras e fornecedores, taxa de serviço, fidelidade, LGPD, reservas), conferência de calculabilidade dos 11 KPIs (K5, K9 e K11 não calculáveis como escritos), metas sem origem, KPIs ausentes de compras e caixa, e itens do esqueleto não atendidos (rastreabilidade, justificativa de processos e requisitos não incluídos, lista de RN).
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 14 — Exportação das correções do documento de regras e KPIs em PNG
- **Ferramenta e etapa:** Claude — transformação da análise do documento do integrante em uma imagem PNG para enviar ao grupo.
- **Prompt:** "exporte essas correções para eu mandar pro outro integrante, export num png".
- **Resposta recebida:** `correcoes_documento_regras_bi.png` com as cinco regras que contradizem o DER, a tabela de calculabilidade dos 11 KPIs, os itens a incluir e a observação sobre a tabela de rastreabilidade.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 15 — Segunda análise do documento de regras e BI (versão corrigida pelo integrante)
- **Ferramenta e etapa:** Claude — conferência da versão corrigida (documento2.docx) contra o DER, o esqueleto e as correções anteriores.
- **Prompt:** "analise agora a correção da parte do outro membro sobre BI e regras de negócio" (anexo: documento2.docx).
- **Resposta recebida:** confirmação de que as contradições anteriores foram resolvidas; ajustes restantes: definição do K1 (inclui quem está na fila), regra de ambiente nos K7 e K10 ("primeira mesa" não calculável), K14 com duas definições, G05 ausente na rastreabilidade do P2, linha de horário de funcionamento/normas de conduta, framing da última aplicação de IA; pendências: lista de RN com códigos, justificativa de processos e requisitos não incluídos, confirmação na entrevista, script com G01 a G16.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 16 — Aplicação dos ajustes no documento de regras e BI
- **Ferramenta e etapa:** Claude — edição do documento do integrante (regras não incluídas e arquitetura analítica 1.6) com os seis ajustes apontados na segunda análise.
- **Motivação:** deixar o texto calculável contra o DER e no padrão de títulos do README.
- **Prompt:** "certo, pode aplicar os ajustes".
- **Resposta recebida:** `regras_e_bi_corrigido.md` com: K1 restrito a acessos com entrada e sem saída; K2 apenas com entrada; K7 e K10 usando o ambiente da mesa de menor `id_mesa`; K14 com uma só definição (qtd × custo_unitario, sem compras canceladas) e consulta de auditoria do `valor_total` das compras; G05 incluído na rastreabilidade do P2; linha de horário de funcionamento, áreas restritas e normas de conduta; última aplicação de IA reescrita para a gerência. Metas continuam propostas, a confirmar na entrevista.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 17 — Confronto das respostas da entrevista com o DER, o dicionário e as regras
- **Ferramenta e etapa:** Claude — leitura das respostas da entrevista (roteiro preenchido, 06/10/26) e comparação com o DER de 19 entidades, o dicionário v2.0 e o documento de regras e BI.
- **Motivação:** validar com o responsável os domínios e processos assumidos e achar o que a realidade do Ponto A muda no modelo.
- **Prompt:** "Temos a entrevista!" (anexo: roteiro_entrevista_ponto_a preenchido).
- **Resposta recebida:** lista de divergências: cliente não é cadastrado e nenhum dado de cliente é guardado (CLIENTE e LGPD), prato em uma só categoria (Classifica passa a 1:N), sem sangria (SANGRIA), bebidas sem vínculo com o estoque (K9 aproximado), pulseira de 0 a 9999 que reinicia (código único só entre as comandas em uso), mais cargos, ambientes e regras novas (cancelamento e estorno só pela gerência, salão só refeições, deck e subsolo só bebidas e fumódromo) e perguntas sem resposta.
- **Fontes / rejeitado / justificativa / reflexão:** arquivo da entrevista, (pode ser visto por completo no [levantamento/levantamento.md](levantamento/levantamento.md))

### Uso 18 — Conferência do DER corrigido e sincronização dos documentos
- **Ferramenta e etapa:** Claude — leitura do DER final (17 entidades, 22 relacionamentos) contra a entrevista e os documentos do grupo; atualização do dicionário (v2.1) e da seção do DER para o README; comparação de tudo o que existe com as tarefas do esqueleto da Entrega 2.
- **Motivação:** garantir que DER, dicionário, seção do README, regras e KPIs descrevam o mesmo modelo antes do modelo lógico.
- **Prompt:** "certo, vamos pegar tudo que temos e comparar com as tarefas da entrega 2. Mandei o DER corrigido também" (anexo: DER.pdf).
- **Resposta recebida:** confirmação de que o DER bate com o dicionário, com o atributo `motivo` acrescentado em MOVIMENTACAO_ESTOQUE; `secao_der.md` e `dicionario_dados.html` atualizados; lista do que falta por item do esqueleto.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 19 — Mapeamento dos processos de negócio e dos processos prováveis não incluídos
- **Ferramenta e etapa:** Claude — organização dos processos do Ponto A a partir da entrevista e da observação presencial, com os dados de cada processo (campos do DER), e levantamento de processos comuns em restaurantes que ficaram de fora.
- **Motivação:** o esqueleto pede os principais processos com os dados envolvidos, a inclusão dos processos que existem na casa e a justificativa dos prováveis não incluídos.
- **Prompt:** "com as informações que temos dá para mapear os processos de negócio também".
- **Resposta recebida:** dez processos (PR01 a PR10), dois deles com situação a confirmar, e uma tabela de nove processos prováveis fora do modelo, com justificativa. Delivery (aplicativo de terceiros) e valet (serviço de apoio) ficaram fora do modelo, por decisão do grupo.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 20 — Análise das regras de negócio e da seção de BI e IA contra a entrevista e o DER final
- **Ferramenta e etapa:** Claude — conferência do documento de regras e BI (documento2.docx, de outro integrante) contra a entrevista, o DER de 17 entidades e o esqueleto da Entrega 2.
- **Motivação:** garantir que regras, KPIs e rastreabilidade descrevam o modelo final e o que o responsável informou.
- **Prompt:** "Vamos ver sobre as regras de negócio e seção de BI e IA. Mandei novamente o arquivo que outro membro fez já que é a parte dele e vamos analisar" (anexo: documento2.docx).
- **Resposta recebida:** lista de trechos que contradizem a entrevista (cliente cadastrado, LGPD, reservas, gorjeta, delivery), KPIs que dependem de campos removidos (K12, sangria no K13, status cancelado em PEDIDO), ajustes pendentes de rodadas anteriores e itens do esqueleto ainda não atendidos (lista de regras com código, KPIs priorizados pela gerência).
- **Fontes / rejeitado / justificativa / reflexão:** anexo de outro integrante do grupo para fins de correção e saber se está no assunto certo

### Uso 21 — Lista de regras de negócio e nova seção de BI e IA
- **Ferramenta e etapa:** Claude — montagem da lista de regras de negócio (RN01 a RN29) a partir da entrevista e da observação, e reescrita da seção 1.6 (informações estratégicas, KPIs, painéis, aplicações de IA e rastreabilidade) sobre o DER final.
- **Motivação:** o esqueleto pede regras com código e dados envolvidos, e KPIs que o banco consiga calcular, com consulta no script e confirmação com o responsável.
- **Prompt:** "vamos montar a lista de RN a partir da entrevista e refazer a parte de BI e IA".
- **Resposta recebida:** `regras_negocio.md` com 29 regras (operacionais, de pagamento e caixa, de estoque e compras, legais) e a tabela das não incluídas; `bi_e_ia_secao_1_6.md` com 16 KPIs (K12 de entrega e a sangria saíram; entrou o consumo por visita e mesa), seis painéis, seis aplicações de IA e rastreabilidade para G01 a G16.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 22 — Verificação do DER e redação da seção 1.2 (conversão para o modelo lógico)
- **Ferramenta e etapa:** Claude — verificação de que o DER final está pronto para a conversão e redação da seção 1.2 (entidades em tabelas, atributos em colunas, relacionamentos em chaves, tratamento das cardinalidades e da generalização).
- **Motivação:** o esqueleto pede, para a conversão, o que virou tabela, coluna, chave, tabela associativa e a estratégia para generalização.
- **Prompt:** "vamos para a seção 1.2, Conversão do Modelo Conceitual para o Modelo Lógico. O diagrama atualizado já está preparado para isso?"
- **Resposta recebida:** confirmação de que o DER não tem generalização, atributos multivalorados nem compostos; texto da seção 1.2 com 19 tabelas (17 entidades e 2 associativas), 24 chaves estrangeiras (20 nas tabelas das entidades e 4 nas associativas) e a decisão de colocar a chave do 1:1 em comanda; registro de que os mínimos (1,n) serão verificados por consultas de auditoria.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 23 — Redação das seções 1.3, 1.4 e 1.5 (chaves, normalização e justificativas técnicas)
- **Ferramenta e etapa:** Claude — lista de PKs e FKs das 19 tabelas, conferência da 1FN, 2FN e 3FN e justificativas técnicas (chaves, restrições com o código da regra, decisões arquiteturais).
- **Motivação:** o esqueleto pede a lista de chaves, o processo de normalização com justificativa de cada forma e a justificativa de cada restrição citando a regra de negócio.
- **Prompt:** "já pode emendar o 1.3, 1.4 e 1.5 que já estão praticamente montados".
- **Resposta recebida:** `secoes_1_3_a_1_5.md` com 19 PKs e 24 FKs; normalização com os casos de cada forma, o preço do momento como fato histórico e três colunas calculáveis tratadas como redundância controlada; tabela de restrições com RN01 a RN29; lista das regras que não viram restrição de banco.
- **Fontes / rejeitado / justificativa / reflexão:** seções já prontas devido as outras terem finalizado e o DER ter sido um dos focos iniciais

### Uso 24 — Análise dos processos de negócio e dos requisitos escritos por outro integrante
- **Ferramenta e etapa:** Claude — revisão dos dois arquivos da seção 1.1 (processos e requisitos) contra o esqueleto da Entrega 2, a entrevista, o DER final, o dicionário v2.1 e as RN/KPIs do grupo.
- **Motivação:** o integrante responsável não sabia se estava na linha certa, se tinha passado do necessário ou se estava bom.
- **Prompt:** "Esses são 2 arquivos que um integrante do grupo responsável pelos processos de negócio e os requisitos fez. Ele está em dúvida se está na linha certa, já passou do necessário ou não está bom. Dê uma analisada, por favor."
- **Resposta recebida:** a versão 2 (somente entrevista, marcadores [E-n]) é a melhor base; a versão 1 passou do necessário (ajustes de DER, perguntas, metas inventadas). Ambas têm trechos desatualizados em relação às decisões posteriores (formas de pagamento, 55 mesas, segurança entrega a pulseira, cancelamento por item, sem `data_hora_entrega`, delivery e valet fora do escopo, status de PEDIDO e COMANDA).
- **Fontes / rejeitado / justificativa / reflexão:** outro integrante do grupo apenas conferindo oq tinha feito e acabou descorindo que passou do "limite"
- **Atualização:** a pedido do grupo, Claude gerou `secao_1_1_processos_e_requisitos_corrigida.md`: processos = PR01–PR10 do grupo; requisitos reescritos a partir da versão 2 do colega (28 RF e 10 RNF), sem metas inventadas além das marcadas como proposta, com tabela de não incluídos, cobertura de P1/P2 e KPIs e cinco pontos a confirmar.

### Uso 25 — Revisão do modelo lógico e do SQL gerado pelo brModelo
- **Ferramenta e etapa:** Claude — revisão dos PDFs do modelo lógico e dos scripts exportados pelo brModelo (seção 2).
- **Motivação:** o assistente de conversão do brModelo colocou chaves estrangeiras do lado errado, criou a tabela `possui` e deixou colunas sobrando.
- **Prompt:** "modelo logico atualizado e o script tambem. Faça a analise agora pfv".
- **Resposta recebida:** lista de correções por rodada (FKs invertidas, colunas residuais, duplicidade de `id_produto` em `prato`, tipos); `roteiro_modelo_logico.md` com as 19 tabelas e as 24 ligações. Os tipos finais ficaram definidos no SQL, porque o brModelo tem poucos tipos.
- **Fontes / rejeitado / justificativa / reflexão:** nenhum(a)

### Uso 26 — Script SQL e massa de dados
- **Ferramenta e etapa:** Claude — geração e teste do `script.sql` (DDL, SELECT com JOIN, UPDATE, DELETE, G01–G16 e auditorias A01–A22) e da `massa_de_dados.sql` (seção 3 e 4).
- **Motivação:** o SQL do brModelo não serve como script final (só tipos INT, chave primária dupla, sem restrições nomeadas, sem regras de negócio).
- **Prompt:** "agora sim colei o script certo".
- **Resposta recebida:** DDL com 19 tabelas, restrições nomeadas e todas as FKs em RESTRICT; massa gerada por script (semana de 26/10 a 01/11/2026: 183 visitas, 1.354 pedidos, 2.427 itens, 240 pagamentos, 82 compras, 458 movimentações); consultas gerenciais ligadas aos KPIs K1–K16; auditorias ligadas às RN. Testado do zero em MariaDB 10.11, sem erro.
- **Fontes / rejeitado / justificativa / reflexão:** O script funciona perfeitamente no mySQLWorkBench 8

---

## 7\. Apresentação Corporativa Final

[Link do video do grupo](https://youtu.be/Agk97OTuins)