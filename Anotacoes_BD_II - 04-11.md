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