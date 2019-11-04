# Anotações da aula 04/11/2019

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
* Gerenciador de recuperação precisa acompanhar o início, término, e se a transação foi confirmada ou abortada.
* **Operações**
	* **_Begin_transaction_** :arrow_right: Marca o início da transação [Entra no estado ativo];
	* **_Read/Write_**;
	* **_End_Transaction_** :arrow_right: Terminou as operações de leitura/escrita [Operação entra no estado de parcialmente confirmada];
	* **_Commit/Abort_** :arrow_right: Termina efetivamente a transação [Entra no estado de efetivada (_commit_) ou de falha (_abort_) e em seguida entra no estado de encerrada].
* Tudo fica no _log_, pois caso tenha um erro, o SGBD vai no _log_ e desfaz as transações (_rollback_).

[Imagem]

### _Log_ do sistema
* Registra todas as operações de transações que afetam valores do banco.
* Cada transação tem um **ID único**.
* Após ter sido abortada pelo SGBD, uma transação volta com o mesmo ID? **Depende**.
* Importante para desfazer ou refazer uma transação.
* Armazenado primeiramente em MP e depois vai para o disco.
	* Gravãção realizada apenas quando o o bloco físico do registro de _log_ estiver preenchido.
	* **Gravação forçada** :arrow_right: de tempos em tempos é feita a gravação do bloco de registros do _log_ mesmo que não esteja preenchido, para que, em caso de erro, o SGBD saiba quais transações foram finalizadas.
* *Control Files*: armazenam todos os dados referentes a gerência do banco.

_OBS: uma insância de um SGBD é a área de memória destinada ao banco, os processos e os arquivos. Em uma mesma máquina pode-se ter mais de uma instância._