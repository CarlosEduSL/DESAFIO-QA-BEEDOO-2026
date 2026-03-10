# DESAFIO-QA-BEEDOO-2026

## 🔍 1. Análise Inicial da Aplicação

Nesta fase inicial, explorei a aplicação para entender o fluxo de navegação e a consistência da interface. Foquei em identificar pontos que podem impactar a experiência do usuário ou a credibilidade do sistema.

### 📋 Observações de UI/UX e Interface

* **Erro de Digitação (Typo):** Identifiquei que no cabeçalho a palavra **"Challenge"** está escrita incorretamente como **"Chalenge"**. Erros desse tipo, embora simples, afetam a percepção de cuidado no desenvolvimento final.
* **Alinhamento e Layout:** Notei que o título "Lista de cursos" e os cards renderizados não possuem um alinhamento centralizado ou uma estrutura de grid definida. O conteúdo parece "solto" e deslocado para a esquerda, o que prejudica a estética em telas maiores.
* **Feedback de Carregamento (Ponto Positivo):** A aplicação utiliza um ícone de carregamento (spinner) enquanto as imagens dos cards são processadas. Isso é uma boa prática de UX, pois informa ao usuário que o sistema está em atividade.
* **Estado Vazio (Empty State):** Ao acessar a listagem sem dados, a tela fica totalmente em branco abaixo do título. Recomenda-se exibir uma mensagem informativa (ex: "Nenhum curso cadastrado") para confirmar que a página carregou corretamente.
* **Feedback de Navegação:** O menu superior não possui um indicador visual (como uma cor diferente) para mostrar qual página está ativa no momento.
* **Ausência de Indicadores de Obrigatoriedade:** Notei que o formulário de cadastro não utiliza sinalizações visuais padrão (como o asterisco `*`) para indicar quais campos são obrigatórios. Isso força o usuário a submeter o formulário para descobrir quais informações são essenciais, o que prejudica a usabilidade (UX).

### ⚙️ Observações Funcionais

* **Fluxo de Exclusão:** Identifiquei a presença do botão **"EXCLUIR CURSO"**. Este é um ponto de atenção crítico: pretendo validar se o sistema solicita uma confirmação do usuário antes de deletar o curso, evitando exclusões acidentais.

### 🎯 Pontos Críticos Estratégicos para Teste

Com base na estrutura do formulário de cadastro, selecionei os seguintes pontos como prioridade para garantir a integridade dos dados:

1. **Lógica de Regras de Negócio (Datas):** Validar se o sistema impede que a "Data de fim" seja configurada para um dia anterior à "Data de início".
2. **Consistência de Dados Numéricos:** Verificar se o campo "Número de vagas" aceita valores negativos, decimais ou caracteres não numéricos.
3. **Validação de URL de Imagem:** Checar se o campo aceita qualquer texto aleatório ou se exige um formato de link válido (http/https) para renderização.
4. **Segurança e Sanitização:** Testar a resistência dos campos de texto contra entradas maliciosas, como injeção de scripts (XSS).

---

## 🐞 3. Relatório de Bugs Encontrados

Abaixo estão detalhados os problemas identificados durante a execução dos testes.

### **BUG-01: Ausência de validação cronológica no campo de datas**
* **Severidade:** Média (Impacta a integridade dos dados e lógica de negócio).
* **Título:** Sistema permite cadastro de cursos com data de término retroativa à data de início.
* **Passos para reproduzir:**
    1. Acessar a tela de cadastro.
    2. Preencher os campos obrigatórios.
    3. Definir a **Data de início** (ex: 10/03/2026) e uma **Data de fim** anterior (ex: 01/03/2026).
    4. Clicar em "CADASTRAR CURSO".
* **Resultado Atual:** O sistema exibe mensagem de sucesso e cadastra o curso com datas logicamente impossíveis.
* **Resultado Esperado:** O sistema deve validar que a data de fim seja posterior à de início e exibir uma mensagem de erro impedindo o cadastro.

### **BUG-02: Falha na validação de valores mínimos (Vagas)**
* **Severidade:** Média (Gera inconsistência nos dados de inventário).
* **Título:** Sistema permite o cadastro de cursos com número de vagas negativo.
* **Passos para reproduzir:**
    1. Acessar a tela de cadastro.
    2. No campo "Número de vagas", inserir um valor negativo (ex: -50) manualmente ou via controles do campo (stepper).
    3. Preencher os demais campos e clicar em "CADASTRAR CURSO".
* **Resultado Atual:** O sistema cadastra o curso com sucesso e exibe o valor negativo no card da listagem.
* **Resultado Esperado:** O sistema deve restringir o campo para aceitar apenas números inteiros e positivos (mínimo 0 ou 1).
