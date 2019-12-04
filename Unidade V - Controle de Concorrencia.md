# Unidade V
---

## Técnicas de Bloqueio
> A transação só acessa um item depois de efetuar o bloqueio.

* Bloqueio binário
    - Podem receber dois valores: **_Locked_ (1)** ou **_Unlocked_ (0)**.
    - Operações
        * **_Lock\_item(X)_**
        * **_Unlock\_item(X)_**
    - Transação permanece em uma fila de espera até que o item seja desbloqueado
    - Bloqueio binário nao faz distição entre operações de leitura e escrita (bloqueia mesmo que todas as operações sejam de leitura)
        
* Bloqueios compartilhados
    - **Características**
        * Permitir que diversas transações acessem o mesmo item se forem apenas de leitura
        * Se a transação for de escrita, o acesso é exclusivo
    - **Operações**
        * **_Read\_lock(X) -> Read-locked_**
            - Bloqueado para leitura
            - **_Share-locked_**
        * **_Write\_lock(X) -> Write-locked_**
            - Bloqueado para gravação
            - **_Exclusive-locked_**

## Conversão de bloqueios
* _**Upgrade**_
    - _Read\_lock(X)_ :arrow_right: Mudar bloqueio para _Write\_lock(X)_.
* _**Downgrade**_
    - _Write\_lock(X)_ :arrow_right: Mudar bloqueio para _Read\_lock(X)_.

## Bloqueios em duas fases
* **1ª Fase** :arrow_right: Expansão ou Crescimento
    * Pode-se obter bloqueio de novos itens, mas nenhum pode ser liberado.
* **2ª Fase** :arrow_right: Encolhimento ou Retrocesso
    * Bloqueios existentes podem ser liberados mas nenhum bloqueio pode ser obtido.

* **Tipos**
    * **Conservador**
        - Requer que uma transação bloqueie todos os itens que acessa antes da transação iniciar
        - Se um item não puder ser bloqueado, então a transação não inicia (ou bloqueia tudo ou bloqueia nada)
        - Livre de **_deadlock_** (mas pode causar **_starvation_**)
    * **Estrito**
        - Transação mão libera bloqueios exclusivos, até que a transação termine (_commit_ ou _abort_)
    * **Rigoroso**
        - Transação não libera bloqueios de gravação nem de leitura antes do _commit_ ou do _abort_
        - Podem causar **_deadlock_** e/ou **_starvation_**

## _Deadlock_
* **Controle de _Deadlock_**
    * **Prevenção de _deadlock_**
        * Evita _deadlocks_ antes qeu ocorram.
        * Preferível se a probabilidade de _deadlocks_ for muito alta.
        1. **Bloqueio de duas fases conservador**
        2. **Ordenar os itens**
            * Transação bloqueia os itens de acordo com a ordem de necessidade.
        3. **Registro de _Timestamp_ da transação**
            * _Timestamp_ Ts(T) -> identificador único atribuído a cada transação.
            * Ordem em que a transação é iniciada.
                * Ex: T1 inicia antes de T2 -> Ts(T1) < Ts(T2)
    * **Detecção e recuperação de _deadlock_**
        * Não evita o _deadlock_, mas o detecta e impede o bloqueio indefinido das transações envolvidas.
        * Mais eficiente se ocorrem poucos _deadlocks_.
    * **Rollback pode ser necessário independente da abordagem utilizada.**
    * **_Wait-die_**
        * Caso Ts(Ti) < Ts(Tj) então Ti mais antiga que Tj e Ti é autorizada a esperar, caso Ti > Tj então Ti é morta e posteriormente reiniciada com o mesmo _timestamp_.
    * **_Wound-wait_**
        * Caso Ts(Ti) < Ts(Tj) então Ti mais antiga que Tj abortar Tj e reiniciá-la com mesmo _timestamp_, caso contrário, Ti é autorizada a esperar.
    * **Por que abortam as transações mais novas?**
        * Desperdiçam menos tempo de processamento (Irão sofrer _rollback_).
        * Podem estar envolvidas em _deadlock_.
* **Detecção de _Deadlock_**
    * É verificado se existe de fato o _deadlock_ e caso positivo, algumas transações envolvidas serão abortadas.
    * Na escolha da transação a ser abortada, deve-se evitar abortar as transações mais antigas.

## _Starvation_


## Protocolos com base em _Timestamp_

## Protocolos multiversão
