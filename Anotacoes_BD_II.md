## Parte 1

### Controle de Concorrência
- Transação Finaliza {*Abort :arrow_right: rollback*, *commit*}

### Problemas de Controle de Concorrência
* **Perda de Atualização**
	Transações acessam um mesmo dado e manipulam seu valor, sendo assim um dos valores está errado.
	[Imagem]
* **Atualização Temporária _(Leitura Suja)_**
	Transação atualiza o valor de um dado e logo depois sofre uma falha.
	[Imagem]
* **Agregação Incorreta**
	Transação calcula uma função de agregação em algumas linhas enquanto que outras transações alteram as mesmas linhas.
	[Imagem]

### Recuperação da Transação
* Após fazer o _commit_, não tem como fazer _rollback_.

### Situações de Falhas
1. **Falha do Computador** (_System Crash_ ou Colapso do Sistema)
	* Erro de _hardware_, _software_ ou rede ocorre durante a transação.

2. **Erro de Transação ou de Sistema**
	* Problema na transação
		* Ex: Excesso de dígitos, dicisão por zero, valores de parâmetros errôneos, usuário interrompe a transação e etc.

3. **Erros locais ou condições de exceção detectados pela transação**
	* Exceções durante a transação que necessite do cancelamento da mesma.
		* Ex: Saldo insuficiente para uma operação bancária.

4. **Imposição de Controle de Concorrência**
	* Abortar uma transação porque viola a seriação ou devido à impasses (_deadlocks_).

5. **Falhas de Disco**
	* Falha em HDs.

6. **Problemas físicos ou catástofres**
	* Falta de energia elétrica, prédio pega fogo e etc.

### Estados da Transação
* Gerenciador de recuperação precisa acompanhar o início, término, e se a transação foi confirmada ou abortada, através de um ID único de cada transação.
* **Operações**
	* **_Begin_transaction_** :arrow_right: Marca o início da transação [Entra no estado ativo];
	* **_Read/Write_**;
	* **_End_Transaction_** :arrow_right: Terminou as operações de leitura/escrita [Operação entra no estado de parcialmente confirmada] - Neste momento o SGBD verifica se existe alguma inconsistência nos dados;
	* **_Commit/Abort_** :arrow_right: Termina efetivamente a transação [Entra no estado de efetivada (_commit_) ou de falha (_abort_) e em seguida entra no estado de encerrada] - Caso tenha alguma inconsistência (Valor sofreu alguma alteração por conta de outra transação), então a operação não pode sofrer _commit_, então sofre _abort_.
* Tudo fica no _log_, pois caso tenha um erro, o SGBD vai no _log_ e desfaz as transações (_rollback_).
* Quando a operação sofre _rollback_ ou _commit_, então a operação não volta, para o estado de _ativo_ e é encerrada. No caso do _abort_, ela **pode** ser incializada novamente pelo SGBD.

__*OBS: Só existe concorrência quando duas ou mais transações querem acessar os mesmos dados.*__

[Imagem]

### _Log_ do sistema
* Registra todas as operações de transações que afetam valores do banco.
* Cada transação tem um **ID único**.
* Após ter sido abortada pelo SGBD, uma transação volta com o mesmo ID? **Depende do protocolo, mas só acontece quando têm-se um impasse, que ocorre apenas quando se tem a concorrência por recursos**.
* Importante para desfazer ou refazer uma transação.
* Armazenado primeiramente em MP e depois vai para o disco.
	* Gravãção realizada apenas quando o o bloco físico do registro de _log_ estiver preenchido.
	* **Gravação forçada** :arrow_right: de tempos em tempos é feita a gravação do bloco de registros do _log_ mesmo que não esteja preenchido, para que, em caso de erro, o SGBD saiba quais transações foram finalizadas.
* *Control Files*: armazenam todos os dados referentes a gerência do banco.
* Alguns SGBDs não permitem o registro de leitura no _log_ (pois ela não altera nada e ocupa muito espaço).
* **Entradas do _log_:**
	1. [_Start transaction, Tid_]
	2. [_Escrever_item, Tid, valor antigo, valor novo_]
	2. [_Ler item, Tid, x_]
	3. [_Commit, Tid_]
	3. [_Abort, Tid_]

**_OBS: uma instância de um SGBD é a área de memória destinada ao banco e os processos. Em uma mesma máquina pode-se ter mais de uma instância. A área "física" do banco de dados (os arquivos físicos) é manipulada pela instância._**

### _Commit_ de uma transação

* **Ponto de _commit_** :arrow_right: transação terminou as operações que podia fazer, e pode sofrer _commit_ (estado parcialmente efetivado), mas ainda não sofreu _commit_.
* **_Commited_** :arrow_right: transação sofreu efetivamente o _commit_, então os dados da transação são copiados da memória para o disco e o _commit_ é registrado no _log_.

### Propriedades da transação

* **Atomicidade** :arrow_right: unidade atômica de processamento.
* **Preservação da Consistência** :arrow_right: execução de uma transação leva o SGBD de um estado consistente para outro, jamais para um estado inconsistente (ex: dados incorretos).
* **Isolamento** :arrow_right: transações concorrentes não devem sofrer interferência entre si (sensação de que as transações estão sendo executadas de modo serial).
* **Durabilidade ou persistência** :arrow_right: transações _commitadas_ devem persistir no disco.
* **ACID**.

<hr>

## Parte 2

* **Conceito de escalonamento** :arrow_right: intercalar operações de transações.
* **Regra:** Não posso trocar a ordem das operações de leitura e escrita nas transações intercaladas (_OBS: no caso, nas operações dentro de um transação_).

### Operações no escalonamento

[imagem]

* **Read** :arrow_right: r(X);
* **Write** :arrow_right: w(X);
* **_Commit_** :arrow_right: c;
* **_Abort_** :arrow_right: a;

### Conflito de operações

* Haverá conflito entre operações se elas satisfazem **três condições ao mesmo tempo**:
	1. Pertencer a **diferentes transações**;
	2. Acessando um **mesmo item**;
	3. Pelo menos uma das operações é de **escrita**.
	
### Escalonamento Completo

1. Contempla as oprações descritas em cada transação e termina com _commit_ **ou** _abort_.
2. **Não** altera a ordem das operações de nenhuma transação.
3. Para quaisquer par de operações conflitantes, umas ocorre antes da outra.

_OBS: 1 e 2 os mais importantes (3 é trivial)._

### Características de um escalonamento recuperável

* Nenhuma transação é confirmada, até que todas as transações anteriores a ela (no escalonamento) forem confirmadas, ou seja, só confirma T2 depois que confirmar T1 [se não, vai de encontro ao C (Consistência dos dados) do ACID].
