<h1 align="center">🚧 Página em Construção 🖥️</h1>

---

## 🌟 Sobre o Projeto CentralBank

O **CentralBank** é um trabalho acadêmico desenvolvido por **Wauclidson Alves Dias**, sob orientação do **Prof. Vítor da Silva Ribeiro**, com o objetivo de ilustrar de forma didática como funciona um sistema bancário completo. Construído inteiramente em **Java 24** e organizado como projeto **Maven**, o CentralBank reúne as principais operações financeiras (contas, saques, depósitos, transferências) e demonstra boas práticas de organização de código, modularidade, persistência em arquivos locais e tratamento de erros no console.

---

## 🚀 O que é o CentralBank?

O CentralBank é uma aplicação de **simulação bancária** que roda em console, permitindo ao usuário explorar:

* **Autenticação de Usuário**: com perfis de Atendente e Administrador, protegidos por login e senha.
* **Abertura de Contas**: criação de novas contas com número automático, dados do titular (nome, CPF) e saldo inicial.
* **Operações de Autoatendimento**: saques, depósitos, consultas de saldo e transferências entre contas, sempre com validações de saldo e existência de conta.
* **Registro de Transações**: todas as movimentações são gravadas em arquivos `.bin` (dados serializados) e em arquivos `.txt` legíveis, formando um histórico completo.
* **Fechamento de Caixa Diário**: geração de relatórios de encerramento que consolidam as transações de cada atendente em arquivos de texto, facilitando auditorias.

Essa estrutura simples, porém robusta, mostra passo a passo como implementar uma lógica bancária clássica em Java, sem depender de frameworks externos — apenas com as bibliotecas padrão do JDK 24 e o plug-in Maven para compilação/execution.

---

## 💡 Funcionalidades Principais

1. **Login Seguro e Perfis de Acesso**

   * **Atendente**: pode realizar operações financeiras de autoatendimento (saque, depósito, consulta, transferência).
   * **Administrador**: cadastra novos atendentes, gera relatórios de fechamento de caixa e gerencia todo o fluxo financeiro.
   * Mensagens claras de erro, como “Senha incorreta” ou “Acesso negado”, para orientar o usuário em caso de tentativa indevida.

2. **Criação e Gerenciamento de Contas**

   * **Cadastro de Conta**: informe nome, CPF e saldo inicial; o sistema gera automaticamente o número sequencial da conta.
   * **Consulta de Saldo**: exibe valor disponível atualizado com data e hora da última transação.
   * **Bloqueio/Desbloqueio de Conta**: administradores podem bloquear contas em situações especiais (ex.: suspeita de fraude).

3. **Operações de Autoatendimento Financeiro**

   * **Saque**: valida se há saldo suficiente antes de autorizar a retirada.
   * **Depósito**: crédito instantâneo na conta informada, com atualização imediata no extrato.
   * **Transferência**: ato de debitar o valor da conta origem e creditar na conta destino, tudo em um fluxo atômico e consistente.
   * **Extrato Simples**: mostra no console o histórico de todas as transações de uma conta, formatadas com data, hora e valores.

4. **Persistência de Transações e Relatórios em Arquivos**

   * **Arquivos Binários (.bin)**: cada objeto **`Conta`** é serializado para garantir que saldo e dados do titular sejam recuperados entre execuções.
   * **Arquivos Texto (.txt)**: cada transação gera uma linha no extrato (por exemplo:

     ```
     [25/05/2025 14:30:55] SAQUE: R$ 200,00 • Conta 1023
     ```

     ), formando um histórico legível que pode ser aberto fora da aplicação.
   * **Fechamento de Caixa Diário**: gera relatórios separados por atendente, contendo quantidade e valores totais de saques, depósitos e saldo final, salvos em `.txt` para conferência.

5. **Menus de Navegação no Console**

   * Listagem de opções numeradas e instruções passo a passo para guiar o usuário.
   * Permite retornar ao menu anterior ou encerrar a aplicação sem precisar digitar comandos complicados.
   * Interface limpa e organizada, mesmo rodando exclusivamente em texto.

6. **Validações e Mensagens Amigáveis**

   * Tratamento de CPF no formato correto (somente números).
   * Verificação de valores negativos para saques/depositar, impedindo operações inválidas.
   * Mensagens detalhadas, como “Conta não encontrada” e “Saldo insuficiente”, auxiliando a correção imediata de erros.

---

## 🔑 Tópicos Relevantes no Código

* **Modelagem de Entidades com Recursos do Java 24**

  * **Registros (records)** para representar uma **`Transacao`**, mantendo o código mais enxuto e garantindo imutabilidade.
  * **Classes seladas (sealed)** para definir hierarquias restritas, garantindo que apenas tipos válidos (saque, depósito, transferência) sejam usados como transações.

* **Camadas de Serviço (Business Logic)**

  * Cada operação bancária (saque, depósito, transferência) está encapsulada em sua respectiva classe **Service**, onde toda a lógica de validação e atualização de dados acontece antes de qualquer gravação em arquivo.
  * Por exemplo, o **`TransacaoService`** verifica existência da conta, checa o saldo mínimo e, só depois, dispara a gravação nos repositórios.

* **Repositórios de Persistência**

  * **`ContaRepository`**: responsável por armazenar e recuperar objetos **`Conta`** em arquivos binários, usando serialização Java.
  * **`TransacaoRepository`**: grava todas as transações em arquivos texto formatados, criando um histórico legível para auditorias e relatórios.
  * Essa separação clara permite, no futuro, trocar a persistência em arquivo por uma camada JDBC ou ORM sem alterar a lógica de negócio.

* **Interface de Usuário (UI) Simples e Clara**

  * As classes de menu (por exemplo, **`MenuAtendente`** e **`MenuAdmin`**) orientam o usuário em cada etapa, lendo a entrada do teclado e chamando os serviços correspondentes.
  * O console exibe saídas formatadas de forma consistente, usando utilitários para data/hora e valores monetários.

* **Build e Execução com Maven**

  * O **`pom.xml`** já vem pré-configurado para compilar em **Java 24** (propriedades `<maven.compiler.source>24</maven.compiler.source>` e `<maven.compiler.target>24</maven.compiler.target>`).
  * Para rodar a aplicação, basta ter o JDK 24 instalado e executar:

    ```
    mvn clean compile exec:java
    ```
  * Não há dependências externas sofisticadas, pois toda a persistência é feita nativamente em arquivos.

---

## 🎁 Bônus: Recursos Extras

1. **Formatadores Customizados**

   * Utilitários que convertem **`LocalDateTime`** em strings no padrão `"dd/MM/yyyy HH:mm:ss"`.
   * Formatação de valores monetários em reais (R\$) com precisão de duas casas decimais, deixando o extrato mais profissional.

2. **Mensagens Coloridas no Console (Opcional)**

   * Uso de códigos ANSI para colorir títulos, avisos de erro e confirmações, tornando a experiência mais interativa e fácil de identificar erros ou sucessos rapidamente.

3. **Javadoc e Comentários Didáticos**

   * Todos os métodos públicos possuem comentários **Javadoc** detalhando finalidade, parâmetros e retorno.
   * Comentários internos explicam escolhas de design (por exemplo, por que usar `record` em vez de classe comum), tornando mais fácil aprender bons padrões de projeto.

4. **Facilidade de Extensão**

   * Quer gerar relatórios em PDF? Crie um novo serviço que leia os arquivos de transação e produza arquivos PDF com bibliotecas como iText.
   * Deseja migrar para banco de dados relacional? Basta criar implementações de repositório usando JDBC ou JPA em substituição aos atuais `ContaRepository` e `TransacaoRepository`.

---

## 🤝 Convite para Explorar o CentralBank

Convido você a **conhecer o código** e **vivenciar cada fluxo** de forma prática:

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/WAUCLIDSON/CentralBank.git
   cd CentralBank
   ```

2. **Compile e execute**:

   ```bash
   mvn clean compile exec:java
   ```

3. **Explore os menus**:

   * Faça login como Atendente ou Administrador.
   * Cadastre contas, realize saques, depósitos, transferências e consulte extratos.
   * Gere o fechamento de caixa e confira como cada transação é armazenada em arquivo.

Dentro do repositório, cada classe apresenta comentários que facilitam o entendimento e permitem que você estude a estrutura de entidades (`record`, `sealed`), serviços com regras de negócio, repositórios para persistência e a interface de console.

---

## 🙌 Agradecimentos

Agradeço ao **Prof. Vítor da Silva Ribeiro** pela orientação durante o desenvolvimento deste projeto e à **Universidade Vale do Rio Doce (UNIVALE)** pelo suporte conceitual, especialmente na disciplina de Programação Orientada a Objetos. Espero que o CentralBank seja um recurso valioso para seus estudos em Java, arquitetura de software e boas práticas de programação.

---

Muito obrigado pela atenção! Se tiver sugestões, dúvidas ou quiser contribuir, abra uma **issue** ou envie um **pull request**. Será um prazer aprimorar ainda mais o CentralBank.

Cordialmente,
**Wauclidson Alves Dias**
