# Processamento e Otimização de Consultas

## Introdução

> Estratégias de execução para capturar resultado da consulta;
> Escolha da estratégia caracteriza _Otimzação da Consulta_;
> Plano de execução escolhido nem sempre é o melhor, mas no geral o SGBD busca a maior eficiência;

##  Etapas do Processamento de uma Consulta

1. Análise sintática/semântica (scanner, passer e validação):

	* Análise de sintaxe;
	* Análise de semântica;
	* Verificação de autorização de acesso.

2. Tradução para uma estrutura de armazenamento bem definida (árvore de consulta);
3. Otimização da consulta;
4. Escolha de um plano de acesso;
5. Geração de código e execução da consulta.

## Tradução de Consultas SQL para Álgebra Relacional

* Decomposição da consulta

	- **OBS:** Primeira árvore nunca está otimizada.
	- **OBS:** Depois da criação da árvore, são seguidos os passos para a otimização da consulta.

	- Blocos de consulta (blocos aninhados são considerados blocos diferentes).

		* **OBS:** Sempre começa pela consulta mais interna.
		* **OBS:** Atributos não utilizados são descartados da memória.

## Otimização de Consultas

Visa minimizar:

> Número de acessos à memória secundária;
> Quantidade de memória principal necessária.

* **OBS:** O tempo gasto na otimização deve ser menor que o tmepo de consulta sem otimização.

## Processo de otimização

- Duas opções:
	1. **Heurística**: Ordenação das operações de acesso ao BD;
	2. **Estimação do custo**: utiliza operações matemáticas.

- Heurística
	
	* Estrutura bem definida de regras:
		1. Principal: **Aplicar o *JOIN* e *PROJECT* o quanto antes.** Pois reduzem o tamanho do arquivo antes de um _JOIN_ ou outra operação binária.
		2. Árvore de consulta:
			- Estrutura de dados de árvore que corresponde a uma expressão de álgebra relacional;
			- Relações de entrada de uma consulta (Tabelas) como nós folha da árvore.
			- Operações da álgebra relacional como nós internos.
			- Nó raíz é o que vai mostrar.
			- **OBS:** Não pode fazer árvore com três tabelas, tem que fazer com duas e o resultado com uma terceira.
	
	* Regras Gerais:
		1. Executar operações de SELEÇÃO o mais cedo possível;
		2. Combinar SELEÇÕES com um PRODUTO CARTESIANO de forma a produzir uma JUNÇÃO NATURAL;
		3. Aplicar operações de PROJEÇÃO de forma antecipada (transferir ou inserir projeções sempre que necessário);
			* Os únicos atributos que devem permanecer num esquema são aqueles que aparecem no resultado ou aqueles necessários para o processamento de consultas subsequentes (_JOIN_).
		4. Aplicar operações em grupo (funções de agregação);
		5.  Procurar subexpressões comuns em uma árvore para computá-las uma única vez e guardá-las em uma tabela temporária.

		**OBS:** Colocar do lado esquerdo, mais inferior, a tabela que retornar um menor número de linhas.

	* Passo-a-passo:
		1. Representação da Consulta;
		2. Transferência das operações SELECT para baixo da árvore de consulta;
		3. Aplicação do SELECT mais restritivo à esquerda na árvore;
		4. Substituindo PRODUTO CARTESIANO e SELECT por JOIN;
		5. Transferência das operações PROJECT para baixo na árvore de consulta.

# Armazenamento no banco de dados e otimização baseada em custo

## Armazenamento de Banco de Dados

* Dados armazenados permanentemente...
	
	1. Grandes para caberem inteiramente na MP (memória principal);
	2. Perda permanente de dados ocorrem menos frequentemente em armazenamento secundário (disco);
	3. Custo de armazenamento menor.

### Disposição de Registros no Disco

* Registro: Conjunto de valores relacionados, no qual cada valor corresponde a um campo de um dado registro
* Arquivo: Conjunto de registros.