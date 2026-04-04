# 2 REFERENCIAL TEÓRICO

---

## 2.1 Compatibilidade de Hardware como Domínio do Problema

A montagem de um computador desktop exige o cruzamento rigoroso de especificações técnicas entre componentes interdependentes. Diferentemente da aquisição de periféricos genéricos, a configuração de um sistema computacional envolve restrições físicas, elétricas e lógicas que, quando violadas, resultam em incompatibilidade absoluta: componentes adquiridos de forma incorreta não funcionam juntos, independentemente de ajustes de software ou configuração. Esse cenário impõe ao usuário leigo um desafio técnico significativo, uma vez que plataformas comerciais tradicionais não oferecem mecanismos de validação preventiva capazes de explicar o motivo de uma incompatibilidade (AMD, 2022; INTEL, 2023).

O presente trabalho concentra-se em cinco categorias de componentes que constituem o núcleo de qualquer configuração desktop: a Unidade Central de Processamento (CPU), a placa-mãe, os módulos de memória RAM, a Unidade de Processamento Gráfico (GPU) e a fonte de alimentação. Cada uma dessas categorias possui especificações técnicas que determinam sua compatibilidade com as demais, formando uma rede de dependências que não pode ser avaliada de forma isolada.

### 2.1.1 Compatibilidade de Soquete CPU-Placa-mãe

A restrição mais fundamental na montagem de um computador é a compatibilidade de soquete entre o processador e a placa-mãe. Cada geração de processador é projetada para um soquete físico específico, definido pelo número, disposição e função de seus contatos elétricos. Essa restrição é binária e absoluta: um processador projetado para o soquete AM5 da AMD é fisicamente incompatível com uma placa-mãe equipada com soquete AM4, e vice-versa, sem qualquer possibilidade de adaptação (AMD, 2022). De forma análoga, processadores Intel para o soquete LGA1851 não são compatíveis com plataformas LGA1700 (INTEL, 2023). A documentação técnica oficial dos fabricantes define formalmente quais combinações de processador e chipset são suportadas, constituindo a principal fonte de restrições binárias do domínio.

### 2.1.2 Padrão de Memória RAM

Os módulos de memória RAM seguem padrões técnicos definidos pela JEDEC (*Joint Electron Device Engineering Council*), organização responsável pela padronização internacional de semicondutores. Os padrões DDR4 e DDR5, por exemplo, diferem em número de pinos, tensão de operação, frequência base e formato físico do encaixe (JEDEC, 2020). Essa diferença torna os dois padrões eletricamente e fisicamente incompatíveis: uma placa-mãe projetada para DDR5 não aceita módulos DDR4, e o próprio conector possui geometria distinta que impede a inserção física incorreta. A seleção de memória compatível depende, portanto, da placa-mãe selecionada, que por sua vez depende do processador — criando uma cadeia de dependências transitivas no grafo de restrições.

### 2.1.3 Capacidade Energética e TDP

Cada componente de hardware possui um indicador de consumo máximo de energia denominado TDP (*Thermal Design Power*), expresso em watts. A fonte de alimentação deve ser dimensionada para suprir a soma dos TDPs de todos os componentes ativos, com uma margem de segurança recomendada de aproximadamente 20% a 30% acima do consumo estimado (ATXSPEC, 2022). Uma fonte subdimensionada provoca instabilidade do sistema, reinicializações inesperadas e, em casos extremos, danos permanentes aos componentes. Essa restrição difere das anteriores por ser de natureza quantitativa: não se trata de uma incompatibilidade binária, mas de uma relação de desigualdade entre a capacidade da fonte e a demanda agregada dos componentes.

### 2.1.4 Formalização como Problema de Restrições

As três categorias de restrições descritas — soquete, padrão de memória e capacidade energética — possuem uma estrutura comum: cada uma envolve um par de componentes e define condições que os valores atribuídos a esse par devem satisfazer. Essa estrutura é precisamente o que a teoria dos Problemas de Satisfação de Restrições (CSP) formaliza: cada categoria de componente pode ser modelada como uma variável, o conjunto de modelos comercialmente disponíveis em cada categoria constitui o domínio dessa variável, e cada regra de compatibilidade física constitui uma restrição sobre pares de variáveis. Essa correspondência torna o CSP não apenas uma metáfora conveniente, mas o arcabouço formal mais adequado para modelar o problema de validação de configurações de hardware, conforme detalhado na seção seguinte.

**Referências desta seção:**
- AMD (2022); INTEL (2023); JEDEC (2020); ATXSPEC (2022)

---

## 2.2 Problemas de Satisfação de Restrições (CSP)

Um Problema de Satisfação de Restrições (CSP) é formalmente caracterizado por uma tripla ⟨X, D, C⟩, na qual X = {X₁, ..., Xₙ} denota um conjunto finito de variáveis, D = {D₁, ..., Dₙ} representa os respectivos domínios, de modo que cada Dᵢ delimita os valores admissíveis para Xᵢ, e C reúne as restrições que impõem condições sobre as combinações válidas de valores (RUSSELL; NORVIG, 2010). Cada restrição Cⱼ é expressa como um par ⟨scope, rel⟩, em que *scope* é uma tupla de variáveis participantes e *rel* define os valores que essas variáveis podem assumir conjuntamente (RUSSELL; NORVIG, 2010). Diz-se que o problema está resolvido quando se obtém uma atribuição completa e consistente: uma função que associa a cada variável um valor de seu domínio sem infringir nenhuma restrição de C.

### 2.2.1 Tipos de Restrições

As restrições em um CSP podem ser classificadas quanto ao número de variáveis que envolvem. Restrições unárias afetam uma única variável, limitando seu domínio independentemente das demais — por exemplo, a restrição de que a fonte de alimentação deve ter potência mínima de 500W. Restrições binárias envolvem exatamente duas variáveis e são as mais comuns em problemas de compatibilidade de hardware — por exemplo, a restrição de que o soquete da CPU deve corresponder ao soquete suportado pela placa-mãe. Restrições de ordem superior envolvem três ou mais variáveis simultaneamente (DECHTER, 2003). O sistema proposto neste trabalho opera predominantemente com restrições binárias, o que viabiliza a representação estrutural em grafo de restrições, formalizada na seção 2.3.

### 2.2.2 Aplicação ao Domínio de Hardware

A aplicação dessa formalização ao domínio de compatibilidade de hardware é direta. Considerando os cinco componentes centrais do sistema, a tripla ⟨X, D, C⟩ pode ser instanciada da seguinte forma:

- **X** = {CPU, Placa-mãe, RAM, GPU, Fonte}
- **D** = {D_CPU = modelos de processadores disponíveis, D_Placa = modelos de placas-mãe disponíveis, D_RAM = módulos de memória disponíveis, D_GPU = modelos de GPU disponíveis, D_Fonte = modelos de fonte disponíveis}
- **C** = {soquete(CPU, Placa-mãe), padrão_memória(Placa-mãe, RAM), tdp_cpu(CPU, Fonte), tdp_gpu(GPU, Fonte)}

Para ilustrar concretamente, considere a atribuição parcial {CPU = Ryzen 5 7600, Placa-mãe = B650, RAM = DDR5-5600}. Essa atribuição é consistente: o Ryzen 5 7600 utiliza soquete AM5, a placa B650 suporta AM5, e o padrão DDR5 é compatível com a plataforma AM5. Contudo, a atribuição {CPU = Ryzen 5 7600, Placa-mãe = B650, RAM = DDR4-3200} viola a restrição padrão_memória, pois placas-mãe com chipset B650 suportam exclusivamente DDR5 (AMD, 2022; JEDEC, 2020). A solução do CSP consiste em encontrar uma atribuição completa — cobrindo todas as cinco variáveis — que satisfaça simultaneamente todas as restrições de C.

### 2.2.3 Representação em Grafo

A principal distinção entre o CSP e as abordagens clássicas de busca em espaço de estados reside na representação fatorada que aquele adota: em vez de tratar o estado como uma entidade atômica e indivisível, o CSP expõe a estrutura interna do problema por meio de variáveis e restrições explícitas (RUSSELL; NORVIG, 2010; KUMAR, 1992). Restrições binárias entre pares de variáveis admitem representação natural como grafo, no qual os vértices correspondem às variáveis e as arestas codificam as restrições. Essa estrutura de grafo de restrições é o objeto central sobre o qual os algoritmos de busca e propagação operam, e é formalizada na seção seguinte.

**Referências desta seção:**
- RUSSELL; NORVIG (2010); KUMAR (1992); DECHTER (2003); AMD (2022); JEDEC (2020)

---

## 2.3 Teoria dos Grafos e Grafo de Restrições

### 2.3.1 Definições Fundamentais

Um grafo é formalmente definido como um par ordenado G = (V, E), onde V é um conjunto finito não vazio de vértices e E ⊆ V × V é um conjunto de arestas (DIESTEL, 2017). Um grafo é dito simples quando não contém laços (arestas de um vértice para si mesmo) nem arestas paralelas (múltiplas arestas entre o mesmo par de vértices). Em um grafo não-direcionado, as arestas não possuem orientação: a aresta {u, v} é idêntica à aresta {v, u}. Dois vértices u e v são ditos adjacentes se existe uma aresta {u, v} ∈ E conectando-os (DIESTEL, 2017).

### 2.3.2 Representações Computacionais

Do ponto de vista computacional, um grafo pode ser representado de duas formas principais (CORMEN et al., 2001). A **matriz de adjacência** é uma matriz A de dimensão |V| × |V|, onde A[i][j] = 1 se existe aresta entre os vértices i e j, e 0 caso contrário. Essa representação permite verificar a existência de uma aresta em tempo O(1), mas exige espaço O(V²), independentemente do número de arestas. A **lista de adjacência** associa a cada vértice uma lista dos seus vizinhos, exigindo espaço O(V + E). Para grafos esparsos — onde o número de arestas é significativamente menor que V² — a lista de adjacência é preferível em termos de eficiência de memória (CORMEN et al., 2001).

O grafo de restrições do sistema proposto é esparso por natureza: com cinco vértices (componentes), o número máximo de arestas seria 10, mas o sistema possui apenas quatro restrições binárias efetivas — soquete entre CPU e Placa-mãe, padrão de memória entre Placa-mãe e RAM, TDP da CPU e Fonte, e TDP da GPU e Fonte. Pares como GPU e RAM, ou CPU e GPU, não possuem restrição física direta e portanto não são conectados por arestas. A lista de adjacência é, portanto, a representação mais adequada para esse grafo.

### 2.3.3 Grafo de Restrições

Dado um CSP com variáveis X e restrições C, o **grafo de restrições** G_C = (X, E) é construído de modo que existe uma aresta (Xᵢ, Xⱼ) ∈ E se e somente se existe alguma restrição em C envolvendo simultaneamente as variáveis Xᵢ e Xⱼ (RUSSELL; NORVIG, 2010). Essa representação explicita a estrutura topológica do problema: vértices isolados correspondem a variáveis sem restrições com outras, e componentes fortemente conectados indicam grupos de variáveis com alta interdependência.

Aplicando essa definição ao sistema proposto, o grafo de restrições possui a seguinte estrutura:

- **Vértices:** CPU, Placa-mãe, RAM, GPU, Fonte
- **Arestas:**
  - (CPU, Placa-mãe) — restrição de soquete
  - (Placa-mãe, RAM) — restrição de padrão de memória
  - (CPU, Fonte) — restrição de TDP do processador
  - (GPU, Fonte) — restrição de TDP da placa de vídeo

Essa estrutura revela que a Fonte é um vértice de alta centralidade no grafo, pois está conectada tanto à CPU quanto à GPU — qualquer alteração no domínio de um desses componentes pode propagar restrições para o domínio da Fonte. A Placa-mãe, por sua vez, é o vértice que medeia a compatibilidade entre CPU e RAM, funcionando como ponto de articulação da rede de restrições. Esse grafo é o objeto sobre o qual os algoritmos de busca e propagação operam, cuja complexidade computacional é analisada na seção seguinte.

**Referências desta seção:**
- DIESTEL (2017); CORMEN et al. (2001); RUSSELL; NORVIG (2010)

---

## 2.4 Complexidade Computacional

A teoria da complexidade computacional ocupa-se da classificação de problemas de decisão segundo os recursos algorítmicos — principalmente tempo de execução e espaço de memória — exigidos para sua resolução (CORMEN et al., 2001). Dentro desse arcabouço, a classe **P** reúne os problemas para os quais existem algoritmos determinísticos de tempo polinomial, considerados eficientes e tratáveis mesmo para entradas de grande escala. A classe **NP**, por sua vez, abrange os problemas cujas soluções propostas podem ser verificadas em tempo polinomial por uma máquina determinística, ainda que não se disponha, até o presente, de algoritmos igualmente eficientes para encontrá-las (SIPSER, 2012). A questão sobre a relação entre essas duas classes — se P = NP — permanece em aberto e constitui o problema não resolvido de maior relevância para a ciência da computação teórica, razão pela qual o Clay Mathematics Institute o incluiu entre os sete Problemas do Milênio (SIPSER, 2012).

O interior da classe NP não é uniforme. A literatura demonstra a existência de subclasses com diferentes graus de dificuldade, incluindo problemas resolúveis em tempo subexponencial por meio de não-determinismo limitado (PAPADIMITRIOU; YANNAKAKIS, 1996). O CSP, contudo, não se enquadra nessas classes intermediárias: situa-se no topo da hierarquia como um problema **NP-completo** (SIPSER, 2012). Um problema é classificado como NP-completo quando pertence a NP e todo problema de NP se reduz a ele em tempo polinomial; essa classificação pode ser estabelecida por redução polinomial a partir do Problema da Satisfabilidade Booleana (SAT), um dos problemas canônicos de NP-completude (CORMEN et al., 2001; SIPSER, 2012). Consequentemente, se qualquer problema NP-completo admitisse solução em tempo polinomial, toda a classe NP colapsaria em P (SIPSER, 2012).

### 2.4.1 Implicações para o Domínio de Hardware

Essa natureza NP-completa impõe restrições severas à arquitetura de qualquer sistema de verificação de compatibilidade de hardware fundamentado em CSP. A primeira limitação é de ordem temporal: a avaliação exaustiva de todas as combinações interdependentes de componentes é inviável na prática em razão da explosão combinatória intrínseca à classe (BORODIN; CASHMAN; MAGEN, 2011). Para ilustrar concretamente, considere que cada categoria de componente possua em média 50 modelos disponíveis no catálogo do sistema. Com cinco categorias — CPU, Placa-mãe, RAM, GPU e Fonte — a avaliação por força bruta implicaria examinar 50⁵ = 312.500.000 atribuições candidatas antes de encontrar uma solução válida no pior caso. Esse número cresce exponencialmente à medida que o catálogo se expande, tornando a abordagem inviável em tempo real.

A segunda limitação é de ordem espacial: estratégias alternativas baseadas no mapeamento estático e exaustivo de regras lógicas condicionais — codificando explicitamente cada restrição de hardware por meio de estruturas if-else ou tabelas de decisão — não contornam o problema, pois a representação de funções lógicas complexas por árvores de decisão pode exigir espaço de tamanho exponencial, conforme demonstrado por Jukna et al. (1999). Diante dessas limitações, a abordagem padrão para tornar CSPs tratáveis na prática é a busca sistemática com retrocesso combinada com técnicas de propagação de restrições, que reduzem o espaço efetivo de busca de forma incremental. Essas técnicas são apresentadas nas duas seções seguintes.

**Referências desta seção:**
- CORMEN et al. (2001); SIPSER (2012); PAPADIMITRIOU; YANNAKAKIS (1996); BORODIN; CASHMAN; MAGEN (2011); JUKNA et al. (1999)

---

## 2.5 Algoritmos de Busca e Backtracking

### 2.5.1 Busca em Espaço de Atribuições

Dado o grafo de restrições e a complexidade NP-completa do CSP, é necessário um algoritmo de busca sistemática que explore o espaço de atribuições de forma inteligente, sem enumerar cegamente todas as combinações possíveis. O ponto de partida é o conceito de **atribuição parcial**: uma função que associa valores a um subconjunto das variáveis, podendo ser progressivamente estendida até uma atribuição completa (RUSSELL; NORVIG, 2010). A ideia central dos algoritmos de busca para CSP é construir a atribuição incrementalmente, verificando consistência a cada passo e abandonando ramos da árvore de busca assim que uma inconsistência é detectada.

### 2.5.2 Backtracking

O algoritmo de **backtracking** é o método fundamental de busca para CSPs (RUSSELL; NORVIG, 2010; KUMAR, 1992). Ele percorre recursivamente a árvore de atribuições: em cada nível da recursão, seleciona uma variável ainda não atribuída, tenta cada valor de seu domínio na ordem disponível, verifica se esse valor é consistente com as restrições envolvendo as variáveis já atribuídas, e recua (*backtracks*) ao encontrar uma inconsistência. O pseudocódigo do algoritmo é apresentado a seguir:

```
função BACKTRACKING-SEARCH(csp):
    retornar BACKTRACK({}, csp)

função BACKTRACK(atribuição, csp):
    se atribuição está completa: retornar atribuição
    var ← SELECIONAR-VARIÁVEL-NÃO-ATRIBUÍDA(csp)
    para cada valor em ORDENAR-VALORES(var, atribuição, csp):
        se valor é consistente com atribuição dado csp:
            adicionar {var = valor} à atribuição
            resultado ← BACKTRACK(atribuição, csp)
            se resultado ≠ falha: retornar resultado
            remover {var = valor} da atribuição
    retornar falha
```

A complexidade de pior caso do backtracking puro é O(dⁿ), onde d é o tamanho máximo dos domínios e n é o número de variáveis — reiterando a inviabilidade para instâncias grandes sem otimizações complementares (RUSSELL; NORVIG, 2010).

### 2.5.3 Forward Checking

Uma primeira melhoria ao backtracking puro é o **forward checking** (DECHTER, 2003). Ao atribuir um valor à variável Xᵢ, o algoritmo imediatamente examina cada variável Xⱼ ainda não atribuída que compartilha uma restrição com Xᵢ, e remove de Dom(Xⱼ) todos os valores que se tornam inconsistentes com a atribuição de Xᵢ. Se algum domínio Xⱼ se tornar vazio, o algoritmo detecta a falha antecipadamente e recua, sem precisar explorar o ramo até o fim.

Aplicando ao domínio de hardware: ao selecionar uma CPU com soquete AM5, o forward checking percorre os vizinhos de CPU no grafo de restrições e remove do domínio de Placa-mãe todos os modelos com soquete incompatível com AM5, antes mesmo de tentar atribuir um valor a Placa-mãe. Essa poda antecipada reduz significativamente o número de atribuições exploradas. Contudo, o forward checking verifica apenas os vizinhos diretos da variável recém-atribuída, sem propagar as consequências dessas remoções para variáveis mais distantes no grafo. A forma mais completa e eficiente de propagação, que garante consistência global na rede, é o algoritmo AC-3, apresentado na seção seguinte.

**Referências desta seção:**
- RUSSELL; NORVIG (2010); KUMAR (1992); DECHTER (2003)

---

## 2.6 Propagação de Restrições e Algoritmo AC-3

A principal técnica para contornar a explosão combinatória é a propagação de restrições, realizada por algoritmos de consistência. O mais consolidado na literatura é o **AC-3** (*Arc Consistency Algorithm 3*), proposto por Mackworth (1977), cujo objetivo é garantir a consistência de arco em redes de restrições binárias (KUMAR, 1992). Diz-se que um arco (Xᵢ, Xⱼ) é consistente quando, para todo valor v ∈ Dom(Xᵢ), existe ao menos um valor w ∈ Dom(Xⱼ) tal que a atribuição {Xᵢ = v, Xⱼ = w} satisfaz a restrição entre as duas variáveis.

O algoritmo mantém uma fila de arcos a examinar e, para cada par de variáveis relacionadas, remove do domínio da variável os valores que não encontram suporte em nenhum valor do domínio da variável vizinha; cada remoção dispara a reinserção dos arcos afetados na fila, propagando a mudança pela rede até o ponto de estabilidade, em que nenhuma redução adicional é possível (RUSSELL; NORVIG, 2010). Assumindo um CSP com n variáveis, domínios de tamanho no máximo d e c restrições binárias, a complexidade de pior caso do AC-3 é O(cd³) (RUSSELL; NORVIG, 2010).

### 2.6.1 Aplicação ao Domínio de Hardware

Para ilustrar a execução do AC-3 no sistema proposto, considere o seguinte cenário passo a passo: o usuário seleciona a CPU Ryzen 5 7600, que utiliza soquete AM5. O motor CSP insere na fila todos os arcos envolvendo CPU. Ao processar o arco (Placa-mãe, CPU), o algoritmo verifica cada modelo em Dom(Placa-mãe) e remove aqueles cujo soquete é incompatível com AM5 — por exemplo, todos os modelos com chipset B450 ou Z490 são eliminados. Essa remoção de valores do domínio de Placa-mãe dispara a inserção do arco (RAM, Placa-mãe) na fila. Ao processar esse arco, o AC-3 verifica cada módulo em Dom(RAM) e remove aqueles incompatíveis com as placas-mãe restantes — como todos os modelos AM5 suportam apenas DDR5, todos os módulos DDR4 são eliminados do domínio de RAM. O processo continua propagando pela rede até que nenhuma redução adicional seja possível. O resultado é que uma única seleção do usuário propaga restrições por toda a rede de componentes, atualizando dinamicamente os domínios disponíveis em tempo real.

Aplicado como pré-processamento ou intercalado ao backtracking, o AC-3 identifica antecipadamente inconsistências locais que, não tratadas, provocariam falhas reiteradas durante a busca global, reduzindo de forma expressiva o espaço efetivo de soluções a explorar. Esse motor de inferência baseado em CSP e AC-3 constitui o núcleo do sistema especialista proposto, enquadrado na seção seguinte.

**Referências desta seção:**
- MACKWORTH (1977); KUMAR (1992); RUSSELL; NORVIG (2010)

---

## 2.7 Sistemas Especialistas

Um Sistema Especialista (SE) é um programa de computador projetado para simular o raciocínio de um especialista humano em um domínio restrito e bem definido (RUSSELL; NORVIG, 2010). Diferentemente de sistemas de propósito geral, um SE concentra seu conhecimento em um domínio específico e é capaz de resolver problemas nesse domínio com nível de competência comparável ao de um especialista humano (JACKSON, 1998). Sua arquitetura é composta por três elementos centrais: a **base de conhecimento**, que armazena fatos e regras sobre o domínio; o **motor de inferência**, que aplica essas regras aos fatos disponíveis para derivar conclusões; e a **interface de explicação**, que permite ao sistema justificar suas respostas ao usuário (RUSSELL; NORVIG, 2010).

O motor de inferência pode operar segundo encadeamento progressivo (*forward chaining*), partindo dos fatos conhecidos em direção a conclusões, ou encadeamento regressivo (*backward chaining*), partindo de uma hipótese e verificando se os fatos disponíveis a sustentam (RUSSELL; NORVIG, 2010). No sistema proposto, o motor opera predominantemente em modo progressivo: a cada seleção de componente pelo usuário, o sistema propaga as restrições a partir desse novo fato para derivar quais opções permanecem válidas.

### 2.7.1 Explanation Facilities

A capacidade de justificar o próprio raciocínio — denominada na literatura de *explanation facility* — é o que distingue fundamentalmente um sistema especialista de um mecanismo de filtragem comum (CLANCEY, 1983). Segundo Clancey (1983), essa capacidade exige que o sistema mantenha não apenas as conclusões derivadas, mas também a cadeia de regras e restrições que levou a cada conclusão, de modo que possa reconstruir e comunicar o raciocínio em linguagem acessível ao usuário. Um mecanismo de filtragem simples responde apenas à pergunta "quais opções são válidas?", apresentando resultados sem elaboração. Um SE com *explanation facility* responde também à pergunta "por que as demais opções são inválidas?", articulando a regra específica que foi violada.

No contexto do sistema proposto, essa capacidade se manifesta da seguinte forma: ao bloquear a seleção de um módulo DDR4 após a escolha de uma CPU AM5, o sistema não se limita a remover o componente da lista de opções — ele apresenta ao usuário uma justificativa explícita como "Este módulo de memória é incompatível: a placa-mãe compatível com o processador selecionado suporta exclusivamente DDR5, e este módulo utiliza o padrão DDR4." Essa explicação transforma cada bloqueio em uma oportunidade de aprendizagem sobre as restrições físicas do hardware.

### 2.7.2 Mapeamento ao Sistema Proposto

O sistema proposto neste trabalho se enquadra na categoria de sistemas especialistas baseados em restrições. Os três componentes arquiteturais mapeiam diretamente à implementação: a **base de conhecimento** corresponde ao banco de dados PostgreSQL que armazena as especificações técnicas dos componentes e as regras de compatibilidade modeladas como restrições do CSP; o **motor de inferência** corresponde ao algoritmo AC-3 implementado na camada de lógica da API NestJS, responsável por propagar restrições e podar domínios a cada interação do usuário; e a **interface de explicação** corresponde aos alertas técnicos e educativos exibidos na interface Next.js, que comunicam ao usuário não apenas quais componentes são bloqueados, mas as razões físicas e técnicas subjacentes a cada bloqueio. Quando essa interface de explicação é projetada com objetivo pedagógico explícito, o sistema especialista aproxima-se do conceito de Sistema Tutor Inteligente, apresentado na seção seguinte.

**Referências desta seção:**
- RUSSELL; NORVIG (2010); JACKSON (1998); CLANCEY (1983)

---

## 2.8 Sistemas Tutores Inteligentes

Um Sistema Tutor Inteligente (STI) é um ambiente computacional que oferece instrução personalizada e adaptada ao perfil do aprendiz, sem a necessidade de intervenção humana direta (WOOLF, 2008). Diferentemente de sistemas de *e-learning* tradicionais, que apresentam o mesmo conteúdo estático a todos os usuários, um STI mantém modelos internos que representam o conhecimento do domínio, o estado do aprendiz e as estratégias pedagógicas disponíveis, ajustando dinamicamente o nível de dificuldade, o tipo de *feedback* e a sequência de conteúdos conforme o desempenho observado (RUSSELL; NORVIG, 2010). Essa personalização é o que confere ao STI sua característica de inteligência: a capacidade de diagnosticar lacunas de conhecimento e de intervir de forma contextualmente adequada.

### 2.8.1 Os Quatro Modelos do STI

A arquitetura formal de um STI é composta por quatro modelos interdependentes, conforme sistematizado por Woolf (2008):

O **modelo do domínio** representa o conhecimento que deve ser ensinado — os conceitos, fatos, procedimentos e relações que compõem o conteúdo da instrução. É a base a partir da qual o sistema determina o que o aprendiz deveria saber em cada etapa. No sistema proposto, o modelo do domínio corresponde às especificações técnicas dos componentes de hardware e às regras de compatibilidade armazenadas no banco de dados, que definem formalmente o conhecimento sobre o qual o sistema instrui o usuário.

O **modelo do estudante** representa o que o aprendiz sabe, o que não sabe e onde estão suas lacunas de conhecimento. STIs sofisticados atualizam esse modelo continuamente com base nas respostas e erros do aprendiz (ANDERSON et al., 1995). No sistema proposto, esse modelo é simplificado: o sistema não mantém um perfil persistente individual do usuário, mas infere implicitamente o nível de conhecimento pelo padrão de erros cometidos durante a seleção de componentes — por exemplo, tentativas repetidas de combinar componentes com padrões de memória incompatíveis sugerem desconhecimento sobre a distinção entre DDR4 e DDR5.

O **modelo pedagógico** define como ensinar — as estratégias de instrução, a sequência de apresentação de conteúdos e os tipos de *feedback* a oferecer em cada situação. No sistema proposto, a estratégia pedagógica adotada é a **explicação reativa**: o sistema não apresenta instrução proativa antes da interação, mas intervém precisamente no momento em que uma incompatibilidade é detectada, apresentando a justificativa técnica em linguagem acessível e contextualizada à escolha específica do usuário.

O **modelo da interface** define como o conteúdo e o *feedback* são apresentados ao aprendiz. No sistema proposto, esse modelo é implementado pela interface *wizard* em Next.js, que mantém visíveis os componentes bloqueados — em vez de simplesmente ocultá-los — acompanhados dos alertas educativos correspondentes. Essa decisão de design é pedagogicamente deliberada: a visibilidade dos componentes incompatíveis, junto com a explicação do motivo do bloqueio, transforma a interface em um mapa das restrições do domínio.

### 2.8.2 Distinção entre Filtragem e Tutoria

A distinção fundamental entre um sistema de filtragem e um sistema tutor reside precisamente na natureza do *feedback* fornecido. Um mecanismo de filtragem tradicional responde à pergunta "quais componentes são compatíveis?", apresentando uma lista de resultados sem qualquer elaboração. Um STI, por sua vez, responde à pergunta "por que esses componentes são ou não compatíveis?", articulando as razões subjacentes à compatibilidade ou incompatibilidade em linguagem acessível ao aprendiz (RUSSELL; NORVIG, 2010). No contexto deste trabalho, o motor CSP cumpre o papel do modelo de domínio do STI: ao propagar restrições e podar domínios de forma incremental, ele não apenas determina quais configurações são válidas, mas também fundamenta as explicações que serão apresentadas ao usuário, tornando cada interação uma oportunidade de aprendizagem sobre os princípios técnicos que regem a montagem de computadores.

**Referências desta seção:**
- WOOLF (2008); RUSSELL; NORVIG (2010); ANDERSON et al. (1995)

---

## 2.9 Arquitetura de Software Orientada a Serviços

### 2.9.1 Arquitetura em Camadas

A arquitetura de software compreende o conjunto de decisões estruturais fundamentais que definem a organização de um sistema, as responsabilidades de seus componentes e as interfaces através das quais eles se comunicam (FOWLER, 2002). Um dos padrões arquiteturais mais consolidados na engenharia de software é a **arquitetura em camadas**, que organiza o sistema em níveis hierárquicos de abstração, cada um com responsabilidades bem delimitadas e comunicando-se apenas com a camada imediatamente adjacente (FOWLER, 2002).

As três camadas clássicas são: a **camada de apresentação**, responsável pela interação com o usuário e pela exibição de informações; a **camada de lógica de negócio**, responsável pelo processamento, pelas regras do domínio e pelas decisões da aplicação; e a **camada de dados**, responsável pelo armazenamento, recuperação e persistência das informações (FOWLER, 2002). Essa separação aumenta a manutenibilidade do sistema — alterações em uma camada não propagam mudanças para as demais — e facilita a testabilidade, permitindo que cada camada seja verificada de forma independente.

### 2.9.2 REST como Estilo Arquitetural

A comunicação entre camadas distribuídas em sistemas web segue estilos arquiteturais que impõem restrições sobre como os componentes interagem. O estilo mais amplamente adotado é o **REST** (*Representational State Transfer*), definido por Fielding (2000) em sua tese de doutorado como um conjunto de seis princípios arquiteturais para sistemas distribuídos baseados em rede.

Os princípios mais relevantes para o sistema proposto são:

O princípio **cliente-servidor** estabelece a separação entre a interface do usuário e o armazenamento e processamento de dados, permitindo que cada lado evolua de forma independente. No sistema proposto, o cliente Next.js e o servidor NestJS evoluem independentemente: a interface pode ser redesenhada sem alterar o motor CSP, e o motor CSP pode ser otimizado sem alterar a interface.

O princípio ***stateless*** (sem estado) estabelece que cada requisição enviada pelo cliente ao servidor deve conter todas as informações necessárias para ser compreendida e processada, sem depender de contexto ou sessão armazenada no servidor entre requisições (FIELDING, 2000). No contexto do sistema proposto, isso significa que cada chamada à API carrega o estado atual completo das seleções do usuário — quais componentes já foram escolhidos e quais permanecem em aberto — e o motor CSP executa a propagação de restrições a partir desse estado, sem manter sessão persistente no servidor. Essa característica garante escalabilidade e simplicidade operacional.

O princípio de **interface uniforme** estabelece que a comunicação entre cliente e servidor deve seguir um contrato padronizado, baseado em recursos identificados por URIs e operações semânticas bem definidas (RICHARDSON; RUBY, 2007). No sistema proposto, cada categoria de componente e cada conjunto de restrições é exposto como um recurso REST, acessível por operações HTTP padronizadas.

### 2.9.3 Mapeamento ao Sistema Proposto

A arquitetura do sistema proposto realiza concretamente os princípios da arquitetura em camadas e do estilo REST. A **camada de apresentação** é implementada em Next.js, que consome a API REST de forma reativa: a cada seleção do usuário, envia uma requisição contendo o estado atual das escolhas e recebe como resposta os domínios atualizados de cada variável, com os componentes incompatíveis identificados e suas justificativas. A **camada de lógica de negócio** é implementada em NestJS, que encapsula o motor de inferência CSP/AC-3 e expõe os resultados via *endpoints* REST — essa camada é responsável por toda a inteligência do sistema, incluindo a propagação de restrições e a geração das explicações educativas. A **camada de dados** é implementada em PostgreSQL com Prisma ORM, que armazena a base de conhecimento: as especificações técnicas dos componentes e as regras de compatibilidade que constituem as restrições do CSP.

Essa arquitetura não é uma escolha arbitrária de tecnologias, mas a realização concreta de princípios de separação de responsabilidades e comunicação sem estado definidos pela literatura de engenharia de software. A separação entre motor de inferência e interface de apresentação, em particular, garante que o conhecimento especialista encapsulado no CSP possa ser reutilizado e expandido independentemente das decisões de design da interface educativa.

**Referências desta seção:**
- FIELDING (2000); FOWLER (2002); RICHARDSON; RUBY (2007)

---

## Referências

AMD. *Socket AM5 Platform Design Guide*. Advanced Micro Devices, 2022. Disponível em: https://developer.amd.com.

ANDERSON, John R. et al. Cognitive Tutors: Lessons Learned. *The Journal of the Learning Sciences*, v. 4, n. 2, p. 167–207, 1995.

ATXSPEC. *ATX Specification Version 3.0*. Intel Corporation, 2022.

BORODIN, Allan; CASHMAN, David; MAGEN, Avner. How well can primal-dual and local-ratio algorithms perform? *ACM Transactions on Algorithms*, v. 7, n. 3, 2011.

CLANCEY, William J. The Epistemology of a Rule-Based Expert System: A Framework for Explanation. *Artificial Intelligence*, v. 20, n. 3, p. 215–251, 1983.

CORMEN, Thomas H. et al. *Introduction to Algorithms*. 2. ed. Cambridge: The MIT Press, 2001.

DECHTER, Rina. *Constraint Processing*. San Francisco: Morgan Kaufmann, 2003.

DIESTEL, Reinhard. *Graph Theory*. 5. ed. Berlin: Springer, 2017. Disponível em: https://diestel-graph-theory.com.

FIELDING, Roy T. *Architectural Styles and the Design of Network-based Software Architectures*. 2000. Tese (Doutorado) — University of California, Irvine, 2000. Disponível em: https://ics.uci.edu/~fielding/pubs/dissertation/top.htm.

FOWLER, Martin. *Patterns of Enterprise Application Architecture*. Boston: Addison-Wesley, 2002.

INTEL. *Intel® Core™ Processors — Technical Specifications*. Intel Corporation, 2023. Disponível em: https://ark.intel.com.

JACKSON, Peter. *Introduction to Expert Systems*. 3. ed. Boston: Addison-Wesley, 1998.

JEDEC. *JESD79-5B: DDR5 SDRAM Standard*. JEDEC Solid State Technology Association, 2020. Disponível em: https://www.jedec.org.

JUKNA, Stasys et al. On P versus NP ∩ co-NP for decision trees and read-once branching programs. *Computational Complexity*, v. 8, n. 4, p. 357–370, 1999.

KUMAR, Vipin. Algorithms for Constraint-Satisfaction Problems: A Survey. *AI Magazine*, v. 13, n. 1, p. 32, 1992.

MACKWORTH, Alan K. Consistency in Networks of Relations. *Artificial Intelligence*, v. 8, n. 1, p. 99–118, 1977.

PAPADIMITRIOU, Christos H.; YANNAKAKIS, Mihalis. On Limited Nondeterminism and the Complexity of the V-C Dimension. *Journal of Computer and System Sciences*, v. 53, n. 2, p. 161–170, 1996.

RICHARDSON, Leonard; RUBY, Sam. *RESTful Web Services*. Sebastopol: O'Reilly, 2007.

RUSSELL, Stuart; NORVIG, Peter. *Artificial Intelligence: A Modern Approach*. 3. ed. Upper Saddle River: Prentice Hall, 2010.

SIPSER, Michael. *Introduction to the Theory of Computation*. 3. ed. Stamford: Cengage Learning, 2012.

WOOLF, Beverly Park. *Building Intelligent Interactive Tutors: Student-Centered Strategies for Revolutionizing E-Learning*. San Francisco: Morgan Kaufmann, 2008.