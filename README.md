# DESAFIO-QA-BEEDOO-2026

## 🔍 1. Análise Inicial da Aplicação

Nesta fase inicial, explorei a aplicação para entender o fluxo de navegação e a consistência da interface. Foquei em identificar pontos que podem impactar a experiência do usuário ou a credibilidade do sistema.

### 📋 Observações de UI/UX (Interface e Experiência)

* **Erro de Digitação (Typo):** Identifiquei que no cabeçalho a palavra **"Challenge"** está escrita incorretamente como **"Chalenge"**. Erros desse tipo, embora simples, podem passar uma impressão de falta de cuidado no desenvolvimento final.
* **Alinhamento e Layout:** Notei que o título **"Lista de cursos"** não possui um alinhamento centralizado ou uma estrutura de grid definida. O texto parece "solto" na tela, o que quebra a hierarquia visual e prejudica a estética da página.
* **Estado Vazio (Empty State):** Ao acessar a listagem sem dados, a tela fica totalmente em branco abaixo do título. Como boa prática de UX, o sistema deveria exibir uma mensagem informativa (ex: "Nenhum curso cadastrado no momento") para que o usuário tenha certeza de que a página carregou corretamente.
* **Feedback de Navegação:** O menu superior funciona, mas não há um indicador visual (como uma cor diferente ou sublinhado) que mostre em qual página o usuário está no momento (página ativa).

