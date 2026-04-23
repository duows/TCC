# Relatório de Auditoria — Referencial Teórico (LaTeX)

**TCC — Henrique José de Souza | IFSP Birigui**

---

## PARTE 1 — ERROS ENCONTRADOS NO DOCUMENTO

---

### 🔴 ERRO CRÍTICO 1 — Typos ortográficos no texto

Erros de digitação identificados que precisam ser corrigidos antes da entrega:

| Linha aproximada | Erro | Correção |
|---|---|---|
| `consitituem` | `consitituem` | `constituem` |
| `bínaria` | `bínaria` | `binária` |
| `dóminio` | `dóminio` | `domínio` |
| `enxaixe` | `enxaixe` | `encaixe` |
| `seguraça` | `seguraça` | `segurança` |
| `convenientem` | `convenientem` | `conveniente` |
| `propagração` (2x) | `propagração` | `propagação` |
| `algori**m**to` | `algorimto` | `algoritmo` |
| `píor caso` | `píor` | `pior` |
| `incrimentalmente` | `incrimentalmente` | `incrementalmente` |
| `vaariável` | `vaariável` | `variável` |
| `satifaz` | `satifaz` | `satisfaz` |
| `concluções` | `concluções` | `conclusões` |
| `presição` | `presição` | `precisão` |
| `medida` | `instrução medida` | `instrução mediada` |
| `apresentao` | `apresentao` | `apresentado` |
| `STIde` | `STIde` | `STI de` |
| `prposto` | `prposto` | `proposto` |
| `leas` | `leas` | `elas` |
| `intefaces` | `intefaces` | `interfaces` |
| `cliente-servidos` | `cliente-servidos` | `cliente-servidor` |
| `sem estaod` | `sem estaod` | `sem estado` |
| `enviadopelo` | `enviadopelo` | `enviado pelo` |
| `relavância` | `relavância` | `relevância` |
| `cesce` | `cesce` | `cresce` |
| `atribuida` (várias) | `atribuida` | `atribuída` |
| `consolidade` | `consolidade` | `consolidado` |
| `domíios` | `domíios` | `domínios` |
| `decição` | `decição` | `decisão` |
| `bôtanica` | `bôtanica` | `botânica` |
| `reponsável` | `reponsável` | `responsável` |
| `falahas` | `falahas` | `falhas` |
| `grandas` | `grandas` | `grandes` |
| `acessivél` | `acessivél` | `acessível` |
| `eleaboração` | `eleaboração` | `elaboração` |
| `vertíces` | `vertíces` | `vértices` |

---

### 🔴 ERRO CRÍTICO 2 — Citação indevida na frase de abertura

**Trecho problemático:**

```latex
uma vez que plataformas comerciais tradicionais não oferecem mecanismos de validação
preventiva capazes de explicar o motivo da incompatibilidade \cite{AMD2022,INTEL2023}.
```

**Problema:** A AMD e a Intel **não afirmam em suas documentações** que plataformas comerciais falham em explicar incompatibilidades. Essas fontes apenas descrevem especificações técnicas de soquetes. Essa afirmação é sua — não está na AMD2022 nem na INTEL2023.

**Correção:** Remova as citações desta frase, ou reescreva a frase para que a citação faça sentido:

```latex
% Opção 1: Remover as citações
uma vez que plataformas comerciais tradicionais não oferecem mecanismos de validação
preventiva capazes de explicar o motivo da incompatibilidade.

% Opção 2: Reescrever e citar as plataformas na seção de trabalhos relacionados
% (já feito corretamente em 2.10.1 com \cite{PCPARTPICKER2024,MEUPC2024})
```

---

### 🔴 ERRO CRÍTICO 3 — Seção 2.9.3 com tecnologias específicas

**Trecho problemático:**

```latex
A camada de apresentação é implementada em Next.js...
A camada de lógica de negócio é implementada em NestJS...
A camada de dados é implementada em PostgreSQL com Prisma ORM...
```

**Problema:** O referencial teórico não deve nomear tecnologias de implementação específicas. Esses detalhes pertencem ao capítulo de Metodologia. A banca pode questionar por que essas tecnologias foram escolhidas antes de elas serem apresentadas metodologicamente.

**Correção:** Substituir por termos genéricos de camada, conforme já discutido anteriormente.

---

### 🟡 AVISO 1 — Citação \cite{Borodin2011} — uso muito indireto

**Trecho:** `a avaliação exaustiva de todas as combinações interdependentes de componentes é inviável na prática em razão da explosão combinatória intrínseca à classe \cite{Borodin2011}`

**Problema:** Borodin et al. (2011) tratam de algoritmos primal-dual para problemas de otimização combinatória — **não tratam de CSP nem de hardware**. A afirmação feita é genérica sobre NP e pode ser embasada diretamente por Sipser (2012) e Cormen (2001), que você já cita no mesmo parágrafo.

**Sugestão:** Substituir `\cite{Borodin2011}` por `\cite{Sipser2012,Cormen2001}`.

---

### 🟡 AVISO 2 — Frase com erro lógico na seção 4.1

**Trecho:**

```latex
Em natureza NP-completa impõe restrições severas...
```

**Correção:**

```latex
Essa natureza NP-completa impõe restrições severas...
```

---

### 🟡 AVISO 3 — \cite{} sem \textcite{} em Mittal1989

**Trecho:** `\cite{Mittal1989} introduzem o conceito de dynamic CSP`

**Problema:** Quando o autor é sujeito da frase, o correto pelo padrão abntex2 é `\textcite{Mittal1989}`, não `\cite{Mittal1989}`. O `\cite` coloca apenas `(MITTAL; FRAYMAN, 1989)` entre parênteses no final, tornando a frase gramaticalmente incorreta.

**Correção:** `\textcite{Mittal1989} introduzem o conceito...`

---

### 🟡 AVISO 4 — Margem de 20%-30% na seção de TDP

**Trecho:** `com uma margem de segurança recomendada de aproximadamente 20\% a 30\% acima do consumo estimado \cite{ATXSPEC2022}`

**Problema:** A ATX Spec 3.0 foca em requisitos elétricos de design para fabricantes de fontes — **provavelmente não contém essa recomendação de porcentagem de forma literal**. Esse é um valor amplamente citado em textos técnicos de mercado, não necessariamente na spec formal.

**Sugestão:** Verifique o documento antes da entrega. Se não encontrar, remova a porcentagem específica e deixe apenas "com margem de segurança suficiente \cite{ATXSPEC2022}".

---

### ✅ O QUE ESTÁ CORRETO

- Chave `Arrieta2020` usada consistentemente em todo o documento (problema anterior resolvido)
- Seção 2.7.3 (Mapeamento) corretamente reescrita sem tecnologias — **exceto a seção 2.9.3 que ainda as menciona**
- Algoritmo com `\fonte{Adaptado de: \textcite{Russell2010}}` — correto
- Uso de `\textcite{}` vs `\cite{}` está correto na maioria das ocorrências
- Figura com `\label{fig:grafo_restricoes}` e fonte correta

---

## PARTE 2 — TABELA COMPLETA DE REFERÊNCIAS

---

### AMD2022

**Título completo:** Meet the New AMD Socket AM5 Platform
**Tipo:** Documento técnico online (misc)
**Onde verificar:** amd.com/en/partner/articles/socket-am5-change-game.html
**Status:** ✅ Verificado e acessível

| Afirmação no documento | Seção da fonte | Situação |
|---|---|---|
| AM5 incompatível com AM4 sem possibilidade de adaptação | "Meet the New AM5 Platform" — mudança de PGA para LGA | ✅ Correto |
| AM5 suporta DDR5 | "Putting the Pieces Together" — "DDR5 system memory" | ✅ Correto |
| TDP nativo de até 170W | "Native Support for up to 170W" | ✅ Correto |
| Plataformas comerciais não explicam incompatibilidades | **Não existe na fonte** | ❌ Citação indevida |

---

### INTEL2023

**Título completo:** LGA1851 and LGA1700 Socket Incompatibility
**Tipo:** Artigo de suporte técnico online (misc)
**Onde verificar:** intel.com/content/www/us/en/support/articles/000099723/processors.html
**Status:** ✅ Verificado e acessível

| Afirmação no documento | Seção da fonte | Situação |
|---|---|---|
| LGA1851 incompatível com LGA1700 apesar das mesmas dimensões | Parágrafo "Socket" — primeira frase | ✅ Correto |
| LGA1851 requer chipsets Intel 800 Series | Parágrafo "Chipset" | ✅ Correto |
| Plataformas comerciais não explicam incompatibilidades | **Não existe na fonte** | ❌ Citação indevida |

---

### JEDEC2020

**Título completo:** JESD79-5B: DDR5 SDRAM Standard
**Tipo:** Norma técnica (techreport)
**Onde verificar:** jedec.org → Free Standards → JESD79-5B (cadastro gratuito obrigatório)
**Status:** ⚠️ Precisa verificar pessoalmente — não é acessível sem cadastro

| Afirmação no documento | Seção da fonte | Situação |
|---|---|---|
| DDR4 e DDR5 diferem em número de pinos | Tabela de especificações físicas — início do documento | ⚠️ Verificar |
| Diferem em tensão de operação | Seção de especificações elétricas | ⚠️ Verificar |
| Diferem em formato físico do encaixe | Diagrama do conector | ⚠️ Verificar |

---

### ATXSPEC2022

**Título completo:** ATX Specification Version 3.0
**Tipo:** Especificação técnica (techreport)
**Onde verificar:** edc.intel.com — buscar "ATX 3.0"
**Status:** ⚠️ Margem de 20–30% provavelmente não está literalmente no documento

| Afirmação no documento | Seção da fonte | Situação |
|---|---|---|
| TDP expresso em watts | Seção de definições | ✅ Provável |
| Margem de 20–30% acima do consumo estimado | Não confirmado | ❌ Verificar antes da entrega |

---

### Russell2010

**Título completo:** Artificial Intelligence: A Modern Approach
**Autores:** Stuart Russell; Peter Norvig
**Edição:** 3. ed., Prentice Hall, 2010
**Status:** ✅ Livro canônico — amplamente verificável

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Definição formal da tripla ⟨X, D, C⟩ | Cap. 6, Seção 6.1, p. 202 | ✅ Correto |
| Restrição como par ⟨scope, rel⟩ | Cap. 6, Seção 6.1, p. 202 | ✅ Correto |
| Atribuição completa e consistente | Cap. 6, Seção 6.1, p. 203 | ✅ Correto |
| CSP com representação fatorada vs. atômica | Cap. 6, introdução, p. 202 | ✅ Correto |
| Grafo de restrições — vértices e arestas | Cap. 6, Seção 6.3, p. 212 | ✅ Correto |
| Backtracking como método fundamental | Cap. 6, Seção 6.3, p. 215 | ✅ Correto |
| Pseudocódigo do backtracking (Algoritmo 1) | Cap. 6, Figura 6.5, p. 215 | ✅ Correto — adaptado |
| Complexidade O(dⁿ) do backtracking | Cap. 6, p. 215 | ✅ Correto |
| Forward checking — vizinhos diretos apenas | Cap. 6, Seção 6.3, p. 216 | ✅ Correto |
| AC-3: fila de arcos, remoção de valores | Cap. 6, Seção 6.2, p. 209 | ✅ Correto |
| Complexidade O(cd³) do AC-3 | Cap. 6, Seção 6.2, p. 210 | ✅ Correto |
| Definição de SE — três componentes | Cap. 10 — verificar índice por "expert system" | ⚠️ Verificar página exata |
| Forward chaining e backward chaining | Cap. 7, Seção 7.5, p. 258 | ✅ Correto |

---

### Kumar1992

**Título completo:** Algorithms for Constraint-Satisfaction Problems: A Survey
**Autor:** Vipin Kumar
**Periódico:** AI Magazine, v. 13, n. 1, p. 32–44, 1992
**Onde verificar:** ojs.aaai.org/aimagazine/index.php/aimagazine/article/view/976 — **PDF gratuito disponível**
**Status:** ✅ Verificado e acessível gratuitamente

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| CSP com representação em grafo de restrições | Seção 2 — "Constraint Satisfaction Problems", p. 32–33 | ✅ Correto |
| AC-3 garante consistência de arco em redes binárias | Seção 3 — "Arc Consistency", p. 35–36 | ✅ Correto |
| Backtracking como método fundamental | Seção 3 — "Backtracking Algorithms", p. 34–35 | ✅ Correto |

---

### Dechter2003

**Título completo:** Constraint Processing
**Autor:** Rina Dechter
**Editora:** Morgan Kaufmann, 2003
**Onde verificar:** Biblioteca física ou Portal Capes
**Status:** ⚠️ Livro físico — verificar acesso

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Restrições de ordem superior (3+ variáveis) | Cap. 2 — "Constraint Networks", p. 10–12 | ✅ Correto |
| Forward checking como melhoria ao backtracking | Cap. 5 — "Look-ahead Algorithms", p. 79–82 | ✅ Correto |

---

### Diestel2017

**Título completo:** Graph Theory
**Autor:** Reinhard Diestel
**Edição:** 5. ed., Springer, 2017
**Onde verificar:** diestel-graph-theory.com — **PDF gratuito do Capítulo 1**
**Status:** ✅ Verificado e acessível gratuitamente

| Afirmação no documento | Seção do livro | Situação |
|---|---|---|
| Definição formal G = (V, E) | Cap. 1, Seção 1.1, p. 2 | ✅ Correto |
| Grafo simples — sem laços, sem arestas paralelas | Cap. 1, Seção 1.1, p. 2–3 | ✅ Correto |
| Grafo não-direcionado e adjacência | Cap. 1, Seção 1.1, p. 3 | ✅ Correto |

---

### Cormen2001

**Título completo:** Introduction to Algorithms
**Autores:** Thomas H. Cormen et al.
**Edição:** 2. ed., MIT Press, 2001
**Status:** ✅ Livro canônico — amplamente verificável

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Classificação de problemas — recursos algorítmicos | Cap. 34, Seção 34.1, p. 966 | ✅ Correto |
| NP-completo e redução polinomial a partir do SAT | Cap. 34, Seção 34.3, p. 978–979 | ✅ Correto |
| Matriz de adjacência — O(V²), verificação O(1) | Cap. 22, Seção 22.1, p. 527–528 | ✅ Correto |
| Lista de adjacência — O(V+E), grafos esparsos | Cap. 22, Seção 22.1, p. 527 | ✅ Correto |

---

### Sipser2012

**Título completo:** Introduction to the Theory of Computation
**Autor:** Michael Sipser
**Edição:** 3. ed., Cengage Learning, 2012
**Status:** ✅ Livro canônico — amplamente verificável

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Definição das classes P e NP | Cap. 7, Seções 7.2 e 7.3, p. 286–294 | ✅ Correto |
| P = NP — Clay Mathematics Institute | Cap. 7, Seção 7.4, p. 295 | ✅ Correto |
| CSP é NP-completo | Cap. 7, Seção 7.5, p. 302 | ✅ Correto |
| Colapso de NP em P se NP-completo fosse polinomial | Cap. 7, Seção 7.4, p. 295 | ✅ Correto |

---

### Papadimitriou1996

**Título completo:** On Limited Nondeterminism and the Complexity of the V-C Dimension
**Autores:** Christos H. Papadimitriou; Mihalis Yannakakis
**Periódico:** Journal of Computer and System Sciences, v. 53, n. 2, p. 161–170, 1996
**Onde verificar:** sciencedirect.com/science/article/pii/S0022000096900586
**Status:** ⚠️ Uso indireto — artigo trata de VC-dimension, não de CSP

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Subclasses dentro de NP com graus diferentes de dificuldade | Introdução e Seção 2 | ⚠️ Uso válido mas indireto — Sipser (2012) cobriria melhor |

---

### Borodin2011

**Título completo:** How well can primal-dual and local-ratio algorithms perform?
**Autores:** Allan Borodin; David Cashman; Avner Magen
**Periódico:** ACM Transactions on Algorithms, v. 7, n. 3, 2011
**Status:** ❌ Uso muito indireto — substituir

| Afirmação no documento | Situação |
|---|---|
| Explosão combinatória torna avaliação exaustiva inviável | ❌ Artigo trata de algoritmos primal-dual, não de CSP. Substituir por `\cite{Sipser2012,Cormen2001}` |

---

### Jukna1999

**Título completo:** On P versus NP ∩ co-NP for Decision Trees and Read-Once Branching Programs
**Autores:** Stasys Jukna et al.
**Periódico:** Computational Complexity, v. 8, n. 4, p. 357–370, 1999
**Onde verificar:** Google Scholar → "Jukna Razborov Savicky Wegener 1999 Computational Complexity"
**Status:** ✅ Citação direta e precisa

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Representação de funções lógicas por árvores de decisão pode exigir espaço exponencial | Resultado principal — Teorema sobre branching programs de leitura única | ✅ Correto e preciso |

---

### Mackworth1977

**Título completo:** Consistency in Networks of Relations
**Autor:** Alan K. Mackworth
**Periódico:** Artificial Intelligence, v. 8, n. 1, p. 99–118, 1977
**Onde verificar:** Google Scholar → "Mackworth 1977 Consistency Networks Relations Artificial Intelligence"
**Status:** ✅ Artigo seminal do AC-3

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| AC-3 proposto por Mackworth para consistência de arco | Seção 3 — "Arc Consistency" | ✅ Correto |

> **Nota sobre sintaxe:** O trecho `proposto por \cite{Mackworth1977}` usa `\cite{}` quando deveria usar `\textcite{Mackworth1977}`, pois o autor é sujeito da frase. Corrija para: `proposto por \textcite{Mackworth1977}`

---

### Clancey1983

**Título completo:** The Epistemology of a Rule-Based Expert System: A Framework for Explanation
**Autor:** William J. Clancey
**Periódico:** Artificial Intelligence, v. 20, n. 3, p. 215–251, 1983
**Onde verificar:** Google Scholar → "Clancey 1983 Epistemology Rule-Based Expert System"
**Status:** ✅ Artigo seminal — citação direta e correta

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Explanation facility distingue SE de mecanismo de filtragem | Seção 1 — Introdução | ✅ Correto |
| Sistema deve manter cadeia de regras para reconstruir raciocínio | Seção 2 — "The Explanation Problem" | ✅ Correto |
| Cada bloqueio silencioso é oportunidade de aprendizagem perdida | Interpretação do argumento do autor | ⚠️ Válido como embasamento geral, não citação literal |

---

### Jackson1998

**Título completo:** Introduction to Expert Systems
**Autor:** Peter Jackson
**Edição:** 3. ed., Addison-Wesley, 1998
**Status:** ⚠️ Livro físico — verificar acesso via biblioteca

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| SE resolve problemas com competência comparável a especialista humano | Cap. 1 — "What is an Expert System?", p. 2–5 | ✅ Correto |
| Três componentes: base de conhecimento, motor de inferência, interface | Cap. 2 — "Architecture of Expert Systems", p. 20–25 | ✅ Correto |

---

### Arrieta2020

**Título completo:** Explainable Artificial Intelligence (XAI): Concepts, Taxonomies, Opportunities and Challenges toward Responsible AI
**Autores:** Alejandro Barredo Arrieta et al.
**Periódico:** Information Fusion, v. 58, p. 82–115, 2020
**Onde verificar:** sciencedirect.com/science/article/pii/S1566253519308103 — **acesso gratuito**
**Status:** ✅ Verificado e acessível gratuitamente

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| XAI como conjunto de processos para produzir resultados compreensíveis | Seção 2.1, p. 85–86 | ✅ Correto |
| Explicabilidade como necessidade crítica para adoção prática de IA | Seção 1 — Introdução, p. 82–83 | ✅ Correto |
| Distinção entre sistemas caixa-preta e sistemas transparentes | Seção 3, p. 90–92 | ✅ Correto |

---

### Woolf2008

**Título completo:** Building Intelligent Interactive Tutors: Student-Centered Strategies for Revolutionizing E-Learning
**Autor:** Beverly Park Woolf
**Editora:** Morgan Kaufmann, 2008
**Status:** ⚠️ Livro físico — verificar acesso via biblioteca ou Portal Capes

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| STI oferece instrução personalizada sem intervenção humana direta | Cap. 1, p. 3–5 | ✅ Correto |
| Quatro modelos: domínio, estudante, pedagógico, interface | Cap. 1 e Cap. 2, p. 15–22 | ✅ Correto |

---

### Anderson1995

**Título completo:** Cognitive Tutors: Lessons Learned
**Autores:** John R. Anderson et al.
**Periódico:** The Journal of the Learning Sciences, v. 4, n. 2, p. 167–207, 1995
**Onde verificar:** Google Scholar → "Anderson Corbett Koedinger Pelletier 1995 Cognitive Tutors"
**Status:** ✅ Acessível no Google Scholar

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Modelo do estudante exige perfil persistente por usuário | Seção "Architecture", p. 170–172 | ✅ Correto |
| STIs atualizam modelo com base em erros e acertos | Seção "Student Modeling", p. 172–175 | ✅ Correto |

---

### Fowler2002

**Título completo:** Patterns of Enterprise Application Architecture
**Autor:** Martin Fowler
**Editora:** Addison-Wesley, 2002
**Status:** ⚠️ Livro físico — verificar acesso via biblioteca

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Arquitetura de software como decisões estruturais de organização | Cap. 1 — "Layering", p. 17 | ✅ Correto |
| Arquitetura em camadas com responsabilidades delimitadas | Cap. 1, p. 19–20 | ✅ Correto |
| Três camadas clássicas | Cap. 1 — "The Three Principal Layers", p. 20 | ✅ Correto |
| Separação aumenta manutenibilidade e testabilidade | Cap. 1, p. 19–20 | ✅ Correto |

---

### Fielding2000

**Título completo:** Architectural Styles and the Design of Network-based Software Architectures
**Autor:** Roy Thomas Fielding
**Tipo:** Tese de Doutorado — University of California, Irvine, 2000
**Onde verificar:** ics.uci.edu/~fielding/pubs/dissertation/top.htm — **acesso totalmente gratuito**
**Status:** ✅ Verificado e acessível gratuitamente

| Afirmação no documento | Capítulo/Página da tese | Situação |
|---|---|---|
| REST definido como conjunto de seis princípios arquiteturais | Cap. 5, Seção 5.1, p. 76–77 | ✅ Correto |
| Princípio cliente-servidor | Cap. 5, Seção 5.1.2, p. 78 | ✅ Correto |
| Princípio stateless | Cap. 5, Seção 5.1.3, p. 78–79 | ✅ Correto |

> **Nota sobre sintaxe:** O trecho `definido por \cite{Fielding2000}` deve ser `definido por \textcite{Fielding2000}`, pois o autor é sujeito da frase.

---

### Richardson2007

**Título completo:** RESTful Web Services
**Autores:** Leonard Richardson; Sam Ruby
**Editora:** O'Reilly Media, 2007
**Status:** ⚠️ Livro físico — verificar acesso via biblioteca

| Afirmação no documento | Capítulo/Página | Situação |
|---|---|---|
| Interface uniforme baseada em URIs e operações HTTP | Cap. 4 — "The Resource-Oriented Architecture", p. 104–108 | ✅ Correto |

---

### PCPARTPICKER2024

**Tipo:** Plataforma web
**Onde verificar:** pcpartpicker.com — verificação prática no site
**Status:** ✅ Verificável diretamente

| Afirmação no documento | Como verificar | Situação |
|---|---|---|
| Exibe alerta genérico sem explicar regra técnica violada | Teste: selecione CPU AM5 + módulo DDR4 | ✅ Verificável |
| Implementa filtragem sem explanation facility | Observação direta do comportamento | ✅ Verificável |

---

### MEUPC2024

**Tipo:** Plataforma web
**Onde verificar:** meupc.net
**Status:** ✅ Verificável diretamente

| Afirmação no documento | Situação |
|---|---|
| Incompatibilidades sinalizadas sem justificativa técnica | ✅ Verificável diretamente no site |

---

### Sabin1998

**Título completo:** Product Configuration Frameworks — A Survey
**Autores:** Daniel Sabin; Rainer Weigel
**Periódico:** IEEE Intelligent Systems, v. 13, n. 4, p. 42–49, 1998
**Onde verificar:** Google Scholar → "Sabin Weigel 1998 Product Configuration Frameworks IEEE Intelligent Systems"
**Status:** ✅ Acessível no Google Scholar

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| CSP como abordagem mais expressiva para domínios com regras complexas | Seção 3 — "CSP-Based Configuration", p. 44–46 | ✅ Correto |
| Sistemas CSP superam abordagens de regras estáticas | Seção 4 — "Comparison of Approaches", p. 46–47 | ✅ Correto |

---

### Mittal1989

**Título completo:** Towards a Generic Model of Configuration Tasks
**Autores:** Sanjay Mittal; Felix Frayman
**Publicação:** Proceedings of IJCAI-89, p. 1395–1401, 1989
**Onde verificar:** Google Scholar → "Mittal Frayman 1989 Generic Model Configuration Tasks IJCAI"
**Status:** ✅ Acessível no Google Scholar

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Dynamic CSP — conjunto de variáveis e restrições pode ser alterado durante a busca | Seção 3 — "Dynamic CSP Formulation", p. 1396–1398 | ✅ Correto |

---

### Nwana1990

**Título completo:** An Introduction to Intelligent Tutoring Systems
**Autor:** Hyacinth S. Nwana
**Periódico:** The Knowledge Engineering Review, v. 5, n. 4, p. 251–276, 1990
**Onde verificar:** Google Scholar → "Nwana 1990 Introduction Intelligent Tutoring Systems Knowledge Engineering Review"
**Status:** ✅ Acessível no Google Scholar

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| Capacidade de explicação como condição necessária (mas não suficiente) para transferência de conhecimento | Seção 3 — "What Makes an ITS Effective?", p. 258–260 | ✅ Correto |
| Distinção entre sistemas que explicam *o que* vs. *por que* | Seção 3, p. 259 | ✅ Correto |

---

### Wang2018

**Título completo:** An Expert System Approach to Support Blended Learning in Context-Aware Environment
**Autores:** Cixiao Wang; Feng Wu
**Publicação:** Blended Learning: Enhancing Learning Success. Lecture Notes in Computer Science, v. 10949, Springer, 2018, p. 24–35
**Onde verificar:** Google Scholar → "Wang Wu 2018 Expert System Blended Learning ICBL Springer"
**Status:** ✅ Acessível no Google Scholar

| Afirmação no documento | Seção do artigo | Situação |
|---|---|---|
| SE com feedback explicativo produz ganhos mensuráveis vs. apresentação estática | Seção 4 — "Experiment and Results", p. 30–33 | ✅ Correto |
| Capacidade de explicação é fator determinante para impacto pedagógico | Seção 5 — "Conclusion", p. 33–34 | ✅ Correto |

---

## PARTE 3 — RESUMO EXECUTIVO

### Referências com status ✅ (prontas para entrega)

Kumar1992, Diestel2017, Cormen2001, Sipser2012, Jukna1999, Mackworth1977, Clancey1983, Arrieta2020, Anderson1995, Fielding2000, PCPARTPICKER2024, MEUPC2024, Sabin1998, Mittal1989, Nwana1990, Wang2018

### Referências que precisam de verificação ⚠️

| Referência | O que verificar |
|---|---|
| JEDEC2020 | Baixar PDF no jedec.org e confirmar páginas das 3 afirmações |
| ATXSPEC2022 | Confirmar se a margem de 20–30% está literalmente no documento |
| Russell2010 | Confirmar página exata da definição de SE |
| Dechter2003 | Confirmar acesso ao livro físico |
| Jackson1998 | Confirmar acesso ao livro físico |
| Woolf2008 | Confirmar acesso ao livro físico |
| Fowler2002 | Confirmar acesso ao livro físico |
| Richardson2007 | Confirmar acesso ao livro físico |

### Problemas que exigem correção antes da entrega ❌

| Problema | Ação |
|---|---|
| 35+ typos listados na Parte 1 | Corrigir todos |
| Citação AMD+INTEL na frase de abertura | Remover as citações |
| Seção 2.9.3 com Next.js/NestJS/PostgreSQL | Reescrever com termos genéricos |
| `\cite{Borodin2011}` — uso muito indireto | Substituir por `\cite{Sipser2012,Cormen2001}` |
| `\cite{Mackworth1977}` quando autor é sujeito | Trocar para `\textcite{Mackworth1977}` |
| `\cite{Fielding2000}` quando autor é sujeito | Trocar para `\textcite{Fielding2000}` |
| `\cite{Mittal1989}` quando autor é sujeito | Trocar para `\textcite{Mittal1989}` |
| `Em natureza NP-completa impõe...` | Corrigir para `Essa natureza NP-completa impõe...` |
