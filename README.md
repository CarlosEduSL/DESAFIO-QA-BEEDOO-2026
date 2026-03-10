# DESAFIO-QA-BEEDOO-2026

## 🔍 1. Análise Inicial da Aplicação

Nesta fase inicial, explorei a aplicação para entender o fluxo de navegação e a consistência da interface. Foquei em identificar pontos que podem impactar a experiência do usuário ou a credibilidade do sistema.

### 📋 Observações de UI/UX (Interface e Experiência)

* **Erro de Digitação (Typo):** Identifiquei que no cabeçalho a palavra **"Challenge"** está escrita incorretamente como **"Chalenge"**. Erros desse tipo, embora simples, podem passar uma impressão de falta de cuidado no desenvolvimento final.
* **Alinhamento e Layout:** Notei que o título **"Lista de cursos"** não possui um alinhamento centralizado ou uma estrutura de grid definida. O texto parece "solto" na tela, o que quebra a hierarquia visual e prejudica a estética da página.
* **Estado Vazio (Empty State):** Ao acessar a listagem sem dados, a tela fica totalmente em branco abaixo do título. Como boa prática de UX, o sistema deveria exibir uma mensagem informativa (ex: "Nenhum curso cadastrado no momento") para que o usuário tenha certeza de que a página carregou corretamente.
* **Feedback de Navegação:** O menu superior funciona, mas não há um indicador visual (como uma cor diferente ou sublinhado) que mostre em qual página o usuário está no momento (página ativa).

### 🎯 Pontos Críticos Estratégicos para Teste

Com base na estrutura do formulário de cadastro, selecionei os seguintes pontos como prioridade para garantir a integridade dos dados:

1. **Lógica de Regras de Negócio (Datas):** Validar se o sistema impede que a "Data de fim" seja configurada para um dia anterior à "Data de início".
2. **Consistência de Dados Numéricos:** Verificar se o campo "Número de vagas" aceita valores negativos, decimais ou caracteres não numéricos, o que poderia gerar erros de processamento.
3. **Validação de URL de Imagem:** Checar se o campo aceita qualquer texto aleatório ou se exige um formato de link válido (http/https) para garantir que a imagem seja renderizada corretamente na listagem.
4. **Segurança e Sanitização:** Testar a resistência dos campos de texto (Nome e Descrição) contra entradas maliciosas, como injeção de scripts (XSS) ou caracteres especiais que possam quebrar o layout.
