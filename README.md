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
* **Falha de Responsividade (Mobile):** Ao simular o uso em dispositivos móveis (ex: iPhone 12 Pro), a aplicação apresenta quebras críticas de layout. O logotipo no cabeçalho sofre truncamento ("Beedoo" torna-se "Be..."), os elementos do menu ficam sobrepostos e o grid não se adapta ao tamanho da tela (*viewport*), indicando a ausência de **Media Queries** adequadas.

### ⚙️ Observações Funcionais

* **Fluxo de Exclusão:** Identifiquei a presença do botão **"EXCLUIR CURSO"**. Inicialmente mapeado como ponto de atenção, os testes revelaram que o sistema não apenas carece de confirmação de segurança, como também falha em persistir a ação (ver BUG-05).

### 🎯 Pontos Críticos Estratégicos para Teste

Com base na estrutura do formulário de cadastro, selecionei os seguintes pontos como prioridade para garantir a integridade dos dados:

1. **Lógica de Regras de Negócio (Datas):** Validar se o sistema impede que a "Data de fim" seja configurada para um dia anterior à "Data de início".
2. **Consistência de Dados Numéricos:** Verificar se o campo "Número de vagas" aceita valores negativos, decimais ou caracteres não numéricos.
3. **Validação de Campos Obrigatórios:** Checar se o sistema permite salvar registros vazios.
4. **Persistência de Dados:** Garantir que as ações de cadastro e exclusão reflitam corretamente no banco de dados.

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

### **BUG-03: Ausência total de validação de campos obrigatórios**
* **Severidade:** Alta (Compromete a integridade do banco de dados).
* **Título:** Sistema permite o cadastro de cursos com todos os campos vazios.
* **Passos para reproduzir:**
    1. Acessar a tela de cadastro.
    2. Não preencher nenhum campo (Nome, Instrutor, Datas, etc).
    3. Clicar em "CADASTRAR CURSO".
* **Resultado Atual:** O sistema exibe mensagem de "Curso cadastrado com sucesso!" e renderiza um card vazio na lista, contendo apenas os labels estáticos.
* **Resultado Esperado:** O sistema deve impedir o envio e sinalizar quais campos são obrigatórios (pelo menos Nome, Instrutor e Datas).

### **BUG-04: Permissão de registros duplicados**
* **Severidade:** Baixa/Média (Gera redundância e poluição de dados).
* **Título:** Sistema não valida a existência de cursos duplicados no cadastro.
* **Passos para reproduzir:**
    1. Acessar a tela de cadastro.
    2. Preencher os campos com dados de um curso já cadastrado anteriormente.
    3. Clicar em "CADASTRAR CURSO".
* **Resultado Atual:** O sistema realiza o cadastro e exibe dois cards idênticos na lista de cursos.
* **Resultado Esperado:** O sistema deve validar se já existe um curso com os mesmos dados (nome/instrutor) e impedir a criação de duplicatas.

### **BUG-05: Falha na exclusão de registros (Falso Positivo)**
* **Severidade:** Crítica (Ação funcional não é persistida).
* **Título:** Sistema exibe mensagem de sucesso na exclusão, mas não remove o registro da base.
* **Passos para reproduzir:**
    1. Acessar a tela de listagem.
    2. Localizar um curso (ex: o card em branco criado anteriormente).
    3. Clicar no botão "EXCLUIR CURSO".
    4. Observar a mensagem de feedback e a permanência do card.
    5. Atualizar a página (F5).
* **Resultado Atual:** O sistema exibe o alerta verde de sucesso, mas o card não é removido da interface e permanece salvo no banco de dados após o recarregamento.
* **Resultado Esperado:** O curso deve ser removido permanentemente da base de dados e desaparecer da listagem imediatamente após a confirmação.

---

## 🛠️ 4. Ferramentas Utilizadas
* **Google Sheets:** Para criação da Matriz de Casos de Teste.
* **Google Drive:** Para organização de evidências (Prints).
* **Chrome DevTools:** Para análise de responsividade mobile e inspeção de elementos.

---

## 🔗 Documentação Complementar

| Documento | Link de Acesso |
| :--- | :--- |
| **Planilha de Casos de Teste** | [Acessar Matriz de Testes](https://docs.google.com/spreadsheets/d/1PkpMztL9h-ajI8Lm9AmewpTbPt6lXNKbwmuCZ9u4dlQ/edit?usp=sharing) |
| **Pasta de Evidências** | [Ver Prints e Provas Visuais](https://drive.google.com/drive/folders/1gdQV54wJAE-WvZICqzMhc2zlg7IK87rf?usp=sharing) |

---

## 📅 5. Status Final da Execução
* **Total de Casos de Teste:** 06
* **Passou:** 01
* **Falhou:** 05
