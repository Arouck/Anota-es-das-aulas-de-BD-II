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