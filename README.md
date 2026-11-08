# Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final

### Modelagem de um sistema de gestão de informações para um restaurante


## 1\. Relatório Técnico Final


### Integrantes

| Nome completo do aluno | RGM | Usuário do GitHub |
| :---- | :---- | :---- |
| Diego Xavier Ribeiro | 47793007 | ytiow-die |
| Luiz Felipe de Brito Carvalho | 47857714 | felipeBrito-369 |
| Nicolas Vieira de Lima | 47212021 | n1kDev |
| Renan Caio de Lima | 46915575 | SnyQ285 |


## 1.1 Revisão do Modelo Conceitual
 
### Caracterização da organização
 
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
 
A equipe tem garçons (11), bar (6), caixa (3), valet (5), cozinha (15), narguilé (3), limpeza (4), sushiman (2), pizzaiolo (2), segurança (5), cumim (4), RH (1), maître (3) e gerente (3). O pedido é feito por aparelho de mão (Palmer) e a casa usa hoje o sistema xMenu.
 
**Problemas e necessidades identificados.**
 
- **P1 – Acesso aos ambientes (Deck).** A entrada no Deck nos horários de pico é por ordem de chegada, conduzida pela gerência. Nada é registrado: não há horário de solicitação, de entrada nem de saída, e o tempo de espera não é medido. A gerência não sabe quanto o cliente espera nem quanto cada ambiente está ocupado.
- **P2 – Controle de estoque de bebidas.** A saída é registrada por requisição do bar, e o pedido só dá uma quantidade aproximada do que saiu. O xMenu não direciona o funcionário na contagem nem controla as doses. A contagem é feita pelos maîtres, que também definem o estoque mínimo, com uma base diária. A quebra é a divergência mais comum.
Os dois problemas foram confirmados pelo responsável como os que a casa quer resolver: "estoque e triagem de mesa".
 
**Escopo do sistema.** O banco reúne os dados de todo o atendimento: pulseira e visita, mesas e ambientes, controle de acesso, pedidos e itens, pagamentos e caixa, cardápio, estoque, compras e fornecedores, e funcionários. Ficam fora do modelo as reservas, a escala da equipe e os registros sanitários, que a administração trata manualmente fora do sistema de atendimento.
 
**Justificativa da escolha.** O Ponto A tem volume alto, vários processos ligados entre si (acesso, pedido, estoque, compra e caixa) e dois problemas de gestão de informação que um banco de dados resolve. Além de ser um lugar onde um dos integrantes do grupo trabalha e convive com esses problemas diariamente e por acaso decidiu dar a ideia para tentarmos resolver essa questão.
 
**Evidências da organização.**
 
- Perfil no Google: [Perfil no maps](https://maps.app.goo.gl/XJJXeGhYZD4S7TNm9)
- Endereço: Rua Dante Pellacani, 192 - Vila Reg. Feijó, São Paulo - SP, 03334-070
- Contato: [site da casa](https://ponto-a.cluvi.com.br), Fone: 01126722292
- Foto da visita: [foto](evidencias/fotoEntrevista.jpg)
**Responsável:** Luiz Eduardo Almeida, maître do Ponto A, telefone 11 91101-7123.
 
---

### Relato da entrevista
 
- **Quem:** Luiz Eduardo Almeida, maître. Entrevistador: Renan Caio de Lima. Outros integrantes presentes: apenas o Renan.
- **Quando e onde:** 06/11/2026, no Ponto A – Restaurante e Pizzaria, no ambiente da organização.
- **Comprovação:** foto da entrevista em [evidencia](evidencias/Fotoentrevista2.jpg).
- **O que foi levantado.** A entrevista teve 39 perguntas sobre porte, atendimento, controle de acesso, estoque, compras, caixa, regras e indicadores. Ela confirmou os dois problemas declarados e trouxe os volumes da casa. Também mudou o modelo em quatro pontos: o cliente não é cadastrado e só a pulseira o identifica, não há sangria no caixa, cada prato fica em uma só categoria e o cancelamento é feito pela gerência, item a item. O levantamento completo está em [levantamento/levantamento.md](levantamento/levantamento.md).
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


**Regras de negócio**

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

#### Pagamento e caixa

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



#### DER conceitual revisado (Caso A)

**Software utilizado:** BRModelo Web, na notação do brModelo (entidades em retângulos, relacionamentos em losangos, atributos em círculos, chaves em círculos preenchidos e cardinalidades mínima e máxima nos dois lados de cada relacionamento). Arquivo: [DER.jpg](DER.jpg).

O DER revisado tem **17 entidades** e **22 relacionamentos**. Ele cobre o atendimento (da pulseira entregue na chegada até o pagamento) e os dois problemas declarados: o controle de acesso aos ambientes (P1) e o controle de estoque de bebidas (P2). Também cobre o caixa e as compras de insumos. Não há chaves estrangeiras no DER; elas aparecem apenas no modelo lógico.

##### Entidades e atributos

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

##### Relacionamentos e cardinalidades

O par (mín,máx) ao lado de cada entidade indica quantas vezes ela participa do relacionamento.

| Relacionamento | Entidade A | Card. A | Card. B | Entidade B | Leitura |
|---|---|---|---|---|---|
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
| Recebe | CAIXA | (0,n) | (1,1) | PAGAMENTO | Todo pagamento é finalizado em um caixa |

##### Revisão do modelo (Caso A)

A tabela registra cada apontamento da correção da Entrega 1 e o que foi feito.

| Apontamento da correção | O que foi feito | Por quê |
|---|---|---|
| `contém` (PEDIDO–PRATO) N:N sem quantidade nem preço praticado | Criada a entidade ITEM_PEDIDO com `qtd`, `preco_unitario` e `observacao`, ligada a PEDIDO (`contem`) e a PRATO (`refere-se a`) | Sem ela não é possível saber quanto de cada prato foi pedido nem o preço cobrado na época |
| PRATO sem nome | Acrescentado `nome_prato` | O cardápio precisa identificar cada prato |
| Sem ambiente, embora o acesso ao Deck seja problema declarado (P1) | Criadas AMBIENTE (`capacidade_maxima`) e CONTROLE_ACESSO (solicitação, entrada, saída, `quant_pessoas`, `status`) | Permite calcular a lotação e o tempo de espera de cada ambiente |
| Sem estoque, embora o controle de bebidas seja problema declarado (P2) | Criadas PRODUTO_ESTOQUE (`qtd_atual`, `estoque_minimo`) e MOVIMENTACAO_ESTOQUE (`tipo`, `qtd`, `data_hora`, `motivo`), com o relacionamento `corresponde a` ligando bebida do cardápio ao produto | Registra toda entrada, saída e ajuste (conferência) e permite sinalizar estoque abaixo do mínimo |
| Caixa (abertura e fechamento) ausente | Criada a entidade CAIXA e o relacionamento `recebe` entre CAIXA e PAGAMENTO | O processo existe no Ponto A e encerra a visita |
| Compra e recebimento de mercadoria, fornecedor ausentes | Criadas FORNECEDOR, COMPRA e ITEM_COMPRA, ligadas ao estoque por `abastece` e `é comprado em` | Garante que a compra seja de fornecedor cadastrado e que a entrada no estoque seja rastreável |
| Cancelamento ausente | Registrado no atributo `status` de ITEM_PEDIDO ("cancelado"); o item permanece no banco | Cancelamento exige histórico, não exclusão, e no Ponto A ele é feito por item |
| Taxa de serviço, cupom fiscal e perda da pulseira sem registro | Acrescentados `valor_taxa_servico` e `numero_nf` em PAGAMENTO e `status` em COMANDA (em uso, baixada ou perdida) | Permitem aplicar e auditar essas regras |
| Atributos sobrepostos em COMANDA_ACESSO | Entidade redesenhada como COMANDA, com `id_comanda`, `codigo` e `status` legíveis | Falha de legibilidade do desenho |
| Mínimo (1,n) impedia mesa nunca usada | MESA passou a (0,n) no relacionamento `Recebe` | O banco deve admitir mesa ainda sem uso |
| Reservas, escala da equipe, normas sanitárias e horário de funcionamento ausentes | **Mantidos fora do modelo.** O responsável confirmou que reservas, escala e registros sanitários são tratados manualmente pela administração, fora do sistema de atendimento | Justificativa de processo não incluído |

##### Mudanças feitas após a entrevista

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

##### Decisões de modelagem

- **VISITA é o eixo do modelo.** Pedidos, pagamentos, comanda e acessos aos ambientes partem dela, e é ela que liga as mesas (N:N, para mesas unidas). O cliente é identificado apenas pela comanda, sem cadastro.
- **FUNCIONARIO é uma única entidade**, com o atributo `cargo`, porque todos os papéis compartilham os mesmos atributos e relacionamentos.
- **ITEM_PEDIDO e ITEM_COMPRA** são entidades próprias, com atributos, no lugar de relacionamentos N:N sem dados, para guardar quantidade e preço praticado de cada item.
- **Os N:N que permanecem** (MESA–VISITA e FUNCIONARIO–PAGAMENTO) viram tabelas associativas no modelo lógico.

**Software utilizado:** BRModelo Web, na notação do brModelo (entidades em retângulos, relacionamentos em losangos, atributos em círculos, chaves em círculos preenchidos e cardinalidades mínima e máxima nos dois lados de cada relacionamento). Arquivo: [DER.jpg](DER.jpg).

O DER revisado tem **17 entidades** e **22 relacionamentos**. Ele cobre o atendimento (da pulseira entregue na chegada até o pagamento) e os dois problemas declarados: o controle de acesso aos ambientes (P1) e o controle de estoque de bebidas (P2). Também cobre o caixa e as compras de insumos. Não há chaves estrangeiras no DER; elas aparecem apenas no modelo lógico.

##### Entidades e atributos

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

##### Relacionamentos e cardinalidades

O par (mín,máx) ao lado de cada entidade indica quantas vezes ela participa do relacionamento.

| Relacionamento | Entidade A | Card. A | Card. B | Entidade B | Leitura |
|---|---|---|---|---|---|
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
| Recebe | CAIXA | (0,n) | (1,1) | PAGAMENTO | Todo pagamento é finalizado em um caixa |

##### Revisão do modelo (Caso A)

A tabela registra cada apontamento da correção da Entrega 1 e o que foi feito.

| Apontamento da correção | O que foi feito | Por quê |
|---|---|---|
| `contém` (PEDIDO–PRATO) N:N sem quantidade nem preço praticado | Criada a entidade ITEM_PEDIDO com `qtd`, `preco_unitario` e `observacao`, ligada a PEDIDO (`contem`) e a PRATO (`refere-se a`) | Sem ela não é possível saber quanto de cada prato foi pedido nem o preço cobrado na época |
| PRATO sem nome | Acrescentado `nome_prato` | O cardápio precisa identificar cada prato |
| Sem ambiente, embora o acesso ao Deck seja problema declarado (P1) | Criadas AMBIENTE (`capacidade_maxima`) e CONTROLE_ACESSO (solicitação, entrada, saída, `quant_pessoas`, `status`) | Permite calcular a lotação e o tempo de espera de cada ambiente |
| Sem estoque, embora o controle de bebidas seja problema declarado (P2) | Criadas PRODUTO_ESTOQUE (`qtd_atual`, `estoque_minimo`) e MOVIMENTACAO_ESTOQUE (`tipo`, `qtd`, `data_hora`, `motivo`), com o relacionamento `corresponde a` ligando bebida do cardápio ao produto | Registra toda entrada, saída e ajuste (conferência) e permite sinalizar estoque abaixo do mínimo |
| Caixa (abertura e fechamento) ausente | Criada a entidade CAIXA e o relacionamento `recebe` entre CAIXA e PAGAMENTO | O processo existe no Ponto A e encerra a visita |
| Compra e recebimento de mercadoria, fornecedor ausentes | Criadas FORNECEDOR, COMPRA e ITEM_COMPRA, ligadas ao estoque por `abastece` e `é comprado em` | Garante que a compra seja de fornecedor cadastrado e que a entrada no estoque seja rastreável |
| Cancelamento ausente | Registrado no atributo `status` de ITEM_PEDIDO ("cancelado"); o item permanece no banco | Cancelamento exige histórico, não exclusão, e no Ponto A ele é feito por item |
| Taxa de serviço, cupom fiscal e perda da pulseira sem registro | Acrescentados `valor_taxa_servico` e `numero_nf` em PAGAMENTO e `status` em COMANDA (em uso, baixada ou perdida) | Permitem aplicar e auditar essas regras |
| Atributos sobrepostos em COMANDA_ACESSO | Entidade redesenhada como COMANDA, com `id_comanda`, `codigo` e `status` legíveis | Falha de legibilidade do desenho |
| Mínimo (1,n) impedia mesa nunca usada | MESA passou a (0,n) no relacionamento `Recebe` | O banco deve admitir mesa ainda sem uso |
| Reservas, escala da equipe, normas sanitárias e horário de funcionamento ausentes | **Mantidos fora do modelo.** O responsável confirmou que reservas, escala e registros sanitários são tratados manualmente pela administração, fora do sistema de atendimento | Justificativa de processo não incluído |

##### Mudanças feitas após a entrevista

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

##### Decisões de modelagem

- **VISITA é o eixo do modelo.** Pedidos, pagamentos, comanda e acessos aos ambientes partem dela, e é ela que liga as mesas (N:N, para mesas unidas). O cliente é identificado apenas pela comanda, sem cadastro.
- **FUNCIONARIO é uma única entidade**, com o atributo `cargo`, porque todos os papéis compartilham os mesmos atributos e relacionamentos.
- **ITEM_PEDIDO e ITEM_COMPRA** são entidades próprias, com atributos, no lugar de relacionamentos N:N sem dados, para guardar quantidade e preço praticado de cada item.
- **Os N:N que permanecem** (MESA–VISITA e FUNCIONARIO–PAGAMENTO) viram tabelas associativas no modelo lógico.
 
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
