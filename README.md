# Revisão de PHP: agenda de contatos

> Formulário para cadastrar contatos (nome, e-mail e telefone) num banco MySQL e página que lista os contatos gravados.

**Contexto:** IFAC · Técnico em Informática para Internet (TIPI) · Revisão de PHP · 3º ano · 2023

## Sobre

Exercício de revisão de PHP feito no início do 3º ano. Começou como uma página que lista os participantes do grupo (`index.php`) e evoluiu para uma pequena agenda: um formulário estilizado com Tailwind CSS envia o contato para `cadastro.php`, que grava na tabela `Contatos` do banco `Agenda`, e `mostrar-contatos.php` exibe todos os registros numa tabela.

O objetivo foi revisar o fluxo formulário → PHP → banco de dados: ler dados enviados por `POST`, conectar ao MySQL com PDO, tratar erros com `try`/`catch` e percorrer o resultado de um `SELECT` com `foreach`.

## Funcionalidades

- **Cadastrar contato** (`form.php` → `cadastro.php`): formulário com e-mail, nome e telefone; o script mostra os dados recebidos e executa um `INSERT` em `Contatos`.
- **Listar contatos** (`mostrar-contatos.php`): faz `SELECT * FROM Contatos` e mostra ID, nome, telefone e e-mail numa tabela, com link de volta para o formulário.
- **Participantes do projeto** (`index.php`): lista os integrantes do grupo e suas idades a partir de dois arrays, usando `count()` e `for`.

## Tecnologias

- PHP 7.4+ com PDO (`pdo_mysql`)
- MySQL ou MariaDB
- HTML5
- Tailwind CSS (Play CDN, com os plugins forms, typography, aspect-ratio e line-clamp)
- Remix Icon 2.5.0 (ícones dos campos)

## Estrutura do projeto

| Caminho | Descrição |
|---|---|
| `form.php` | Formulário de cadastro de contato (envia para `cadastro.php`). |
| `cadastro.php` | Recebe o formulário, mostra os dados e insere o contato no banco. |
| `mostrar-contatos.php` | Lista todos os contatos da tabela `Contatos`. |
| `index.php` | Lista dos participantes do projeto. Também contém um bloco de conexão e `INSERT` da primeira versão (ver Observações). |

## Como rodar

### Pré-requisitos

- PHP 7.4 ou superior com a extensão `pdo_mysql`, **ou** o XAMPP.
- MySQL ou MariaDB em `localhost`, usuário `root` com senha vazia (padrão do XAMPP). Se o seu banco usa outra senha, altere a variável `$password` nos arquivos PHP.
- Conexão com a internet (Tailwind e Remix Icon vêm de CDN).

### Passo a passo

1. Clone o repositório:

   ```bash
   git clone https://github.com/itskelme/ifac-tipi-revisao-php.git
   cd ifac-tipi-revisao-php
   ```

2. Crie o banco e a tabela. Os nomes usam maiúscula inicial (`Agenda`, `Contatos`), igual ao código; em MySQL no Linux essa diferença importa.

   ```sql
   CREATE DATABASE IF NOT EXISTS Agenda
     CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

   USE Agenda;

   CREATE TABLE Contatos (
     id       INT AUTO_INCREMENT PRIMARY KEY,
     nome     VARCHAR(100),
     telefone VARCHAR(20),
     email    VARCHAR(150)
   );
   ```

3. Inicie o servidor:

   ```bash
   php -S localhost:8000
   ```

   No XAMPP, copie a pasta para `C:\xampp\htdocs\`, inicie Apache e MySQL e use `http://localhost/ifac-tipi-revisao-php/` como base dos endereços abaixo.

## Como usar

1. Abra http://localhost:8000/form.php, preencha e-mail, nome e telefone e clique em **Registrar contato**.
2. Abra http://localhost:8000/mostrar-contatos.php para ver os contatos gravados.
3. http://localhost:8000/ mostra a lista de participantes.

## Observações

O código foi mantido como entregue. Problemas que impedem o funcionamento correto ou que valem atenção:

- **`cadastro.php` lê `$_post` em minúsculas.** Superglobais do PHP diferenciam maiúsculas, então `$_post` não existe: o contato é gravado com nome, e-mail e telefone vazios. A correção é trocar por `$_POST`.
- **SQL injection.** `cadastro.php` e `index.php` montam o `INSERT` concatenando os valores do formulário direto na string SQL. O certo é usar `prepare()` com parâmetros, como em `mostrar-contatos.php`.
- **`index.php` grava no banco a cada acesso.** O bloco de `INSERT` da primeira versão continua lá e lê `$_POST['nome']`, `$_POST['telefone']` e `$_POST['email']`, que nunca são enviados; cada visita insere uma linha vazia em `Contatos` e mostra avisos de índice indefinido. Além disso, o arquivo não tem `<head>`, então o Tailwind não é carregado e a página aparece sem estilo.
- Os valores são impressos sem `htmlspecialchars()`.
- `form.php` tem o doctype malformado (`<!DOCTYPE html lang="en">` e `</!DOCTYPE>` no fim), o que o navegador tolera.

## Referências

- [PHP: PDO](https://www.php.net/manual/pt_BR/book.pdo.php)
- [PHP: `PDO::prepare`](https://www.php.net/manual/pt_BR/pdo.prepare.php) (forma segura de fazer o `INSERT`)
- [PHP: superglobais e `$_POST`](https://www.php.net/manual/pt_BR/language.variables.superglobals.php)
- [PHP: exceções (`try`/`catch`)](https://www.php.net/manual/pt_BR/language.exceptions.php)
- [Tailwind CSS: Play CDN](https://tailwindcss.com/docs/installation/play-cdn)
- [Remix Icon](https://remixicon.com/)

## Autores

- Diego Kelme Santos da Silva — [@itskelme](https://github.com/itskelme)

## Licença

Este projeto está sob a licença [CC BY-NC 4.0](LICENSE): uso pessoal e modificação permitidos com atribuição; uso comercial proibido.
