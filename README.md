# ⚡ Sistema de Gestão de Contas de Energia

Este projeto é uma aplicação desktop completa para Windows, desenvolvida em **C#** com **Windows Forms**. 
O sistema simula um ambiente de gerenciamento para uma companhia de energia, permitindo o controle de consumidores e suas faturas mensais. Mais do que uma aplicação funcional, este software foi concebido como um estudo de caso prático sobre 
**Arquitetura de Software em Camadas** e a aplicação de **Padrões de Design (Design Patterns)** para criar um código robusto, desacoplado e de fácil manutenção.

## ✨ Funcionalidades Principais

-   👥 **Gestão de Consumidores:** Cadastro, busca e gerenciamento de clientes, com distinção entre Pessoa Física (CPF) e Pessoa Jurídica (CNPJ).
-   📄 **Lançamento de Contas:** Registro de novas faturas de energia (Residencial ou Comercial) vinculadas a um consumidor.
-   🧮 **Cálculo de Fatura:** O sistema calcula automaticamente o consumo em kWh e o valor total da conta, aplicando tarifas e impostos específicos para cada modalidade (residencial/comercial).
-   🔍 **Consulta Integrada:** Permite buscar um consumidor por seu documento e visualizar de forma clara todas as suas informações e histórico de faturas.
-   📊 **Emissão de Relatórios:** Módulo dedicado para a geração de relatórios (funcionalidade em desenvolvimento).

---

## 🏗️ Arquitetura do Sistema

A aplicação foi projetada utilizando a **Arquitetura em Três Camadas (3-Tier Architecture)**, um padrão que promove a separação de responsabilidades e o baixo acoplamento entre os diferentes componentes do software.
```
┌───────────────────────────┐
│ Camada de Apresentação (UI) │  <-- (Windows Forms: UserControls)
└─────────────┬─────────────┘
│ (Interage com)
┌─────────────▼─────────────┐
│ Camada de Negócios (BLL)    │  <-- (Models, Factories, Services)
└─────────────┬─────────────┘
│ (Interage com)
┌─────────────▼─────────────┐
│ Camada de Acesso a Dados(DAL)│  <-- (Repository, SQLite)
└───────────────────────────┘
```

1.  **Camada de Apresentação (UI):** Responsável por toda a interação com o usuário. Construída com Windows Forms, utiliza `UserControl` para cada tela, que são dinamicamente carregados em um `Form` principal, simulando uma experiência de *single-page application*.
2.  **Camada de Lógica de Negócios (BLL):** Onde residem as regras de negócio, os modelos e a lógica central da aplicação. É independente da UI e da forma como os dados são armazenados.
3.  **Camada de Acesso a Dados (DAL):** Responsável pela comunicação com o banco de dados (leitura e escrita). Abstrai completamente a complexidade da persistência de dados do resto do sistema.

### Padrões de Design Aplicados

-   **Factory Method:** A classe `ContaFactory` é utilizada para criar instâncias de `ContaResidencial` ou `ContaComercial`. A BLL delega a responsabilidade da criação de objetos, tornando o sistema mais flexível e desacoplado.
-   **Strategy:** As classes `ContaResidencial` e `ContaComercial` representam "estratégias" de cálculo diferentes. Ambas implementam a interface definida em `ContaBase`, mas cada uma possui sua própria fórmula para tarifas e impostos, permitindo que a estratégia de cálculo seja alterada sem impactar quem a utiliza.
-   **Repository:** As classes `ConsumidorDB` e `ContaDB` implementam o padrão Repository. Elas centralizam e encapsulam a lógica de acesso aos dados, fornecendo uma interface limpa para a camada de negócios e escondendo os detalhes do SQL e do banco de dados SQLite.

---

## 💡 Princípios de POO em Ação

Os pilares da Programação Orientada a Objetos foram a base para o design do sistema:
-   **Abstração:** As classes `Consumidor` e `ContaBase` definem contratos e comportamentos essenciais, abstraindo os detalhes que são específicos de suas classes filhas.
-   **Herança:** `PessoaFisica` e `PessoaJuridica` herdam de `Consumidor`, reutilizando propriedades comuns. Da mesma forma, `ContaResidencial` e `ContaComercial` herdam de `ContaBase`.
-   **Polimorfismo:** O sistema trata objetos de `PessoaFisica` e `PessoaJuridica` de forma intercambiável através da referência da classe base `Consumidor`, simplificando a lógica em várias partes do código.
-   **Encapsulamento:** Cada classe protege seus dados e expõe apenas as operações necessárias, garantindo que a lógica interna (como os comandos SQL no repositório) permaneça isolada e segura.

---

## 🛠️ Stack Tecnológico

-   **Linguagem:** C# (.NET Framework)
-   **Interface Gráfica:** Windows Forms (WinForms)
-   **Banco de Dados:** SQLite (local, em arquivo)
-   **Driver do Banco de Dados:** `Microsoft.Data.Sqlite`

---

## 🚀 Pré-requisitos e Instalação

Para executar este projeto em sua máquina local, você precisará de:

-   [Visual Studio 2022](https://visualstudio.microsoft.com/pt-br/vs/) (ou superior) com a carga de trabalho ".NET Desktop Development".
-   .NET Framework (versão compatível com o projeto).

**Passos para execução:**

1.  Clone o repositório: `git clone https://github.com/Luis-Sampaio/Trabalho-Consumo-de-Energia`
2.  Abra o arquivo de solução (`ControleDeLuz.sln`) no Visual Studio.
3.  O Visual Studio irá restaurar automaticamente os pacotes NuGet necessários (como o `Microsoft.Data.Sqlite`).
4.  Pressione `F5` ou clique em "Start" para compilar e executar a aplicação.

---

## ▶️ Como Usar

1.  **Inicie a aplicação:** O menu principal será exibido.
2.  **Cadastre um Consumidor:** Vá para a tela de cadastro, escolha entre Pessoa Física ou Jurídica, preencha os dados e salve.
3.  **Cadastre uma Conta:** Na tela de cadastro de conta, busque o consumidor recém-criado pelo seu CPF/CNPJ.
4.  **Preencha os Dados da Leitura:** Informe a leitura anterior e a atual em kWh.
5.  **Calcule e Salve:** Clique em "Calcular" para ver o consumo e o valor total. Em seguida, clique em "Salvar" para registrar a fatura.
6.  **Consulte:** Use a tela de consulta para buscar o consumidor e ver a fatura recém-cadastrada em seu histórico.

---

## 📂 Estrutura do Projeto

```
/
├── ControleDeLuz/                  # Namespace principal (BLL e DAL)
│   ├── Models/                     # Classes de domínio (implícito)
│   │   ├── Consumidor.cs
│   │   ├── PessoaFisica.cs
│   │   └── ...
│   ├── DAL/                        # Camada de Acesso a Dados (implícito)
│   │   ├── ConsumidorDB.cs
│   │   └── ContaDB.cs
│   └── Factories/                  # Padrão de projeto (implícito)
│       └── ContaFactory.cs
│
├── Telas_UserControl/              # Camada de Apresentação (UI)
│   ├── MenuPrincipalControl.cs
│   ├── CadastrarConsumidorControl.cs
│   └── ...
│
├── Form1.cs                        # Janela principal que hospeda os UserControls
└── Program.cs                      # Ponto de entrada da aplicação
```


---

## 👨‍💻 Autores

-   Igor Maia
-   Victor Schneider
-   Luis Sampaio
