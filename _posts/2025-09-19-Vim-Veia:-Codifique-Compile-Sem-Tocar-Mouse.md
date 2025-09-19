---
layout: post
title: "Vim na Veia: Codifique e Compile Sem Tocar no Mouse"
date: 2025-09-19
categories: [tecnologia, código aberto]
description: Descubra como usar o Vim para escrever, compilar e executar seus códigos diretamente pelo terminal no GNU/Linux.
---

---

## Parte 1: O que é o Vim?

---

O **Vim (Vi IMproved)** é um editor de texto poderoso, altamente configurável e projetado para ser usado com máxima eficiência — tudo pelo teclado. Criado por Bram Moolenaar em 1991, ele é uma evolução do editor **Vi**, amplamente usado nos sistemas Unix.

Mesmo com o surgimento de editores modernos como VS Code e Sublime Text, o Vim continua sendo o favorito de muitos desenvolvedores por sua leveza, rapidez e flexibilidade.

---

## Parte 2: Por que Usar o Vim?

---

### 🛠️ Recursos que destacam o Vim:

- **Modos de operação** (Normal, Inserção, Visual) que separam navegação de edição.
- **Atalhos de teclado poderosos** que aceleram o fluxo de trabalho.
- **Altamente personalizável** via `.vimrc` e plugins.
- **Disponível em qualquer terminal**, ideal para servidores, containers ou desenvolvimento remoto.

---

## Parte 3: Instalando o Vim no GNU/Linux

---

O Vim está disponível na maioria das distribuições Linux. Veja como instalar em algumas delas:

### Ubuntu/Debian

```bash
sudo apt update
sudo apt install vim
```
### Fedora

```bash
sudo dnf install vim
```
### Arch Linux

```bash
sudo pacman -S vim
```

## Outras distribuições
### OpenSUSE:

```bash
sudo zypper install vim
```
### Gentoo:

```bash
sudo emerge app-editors/vim
```
Verifique a instalação com:

```bash
vim --version
```
---

## Parte 4: Configurando o Vim para Programar

---

Crie ou edite seu arquivo de configuração:

```bash
vim ~/.vimrc
```

Sugestões de configuração:
```vim
Copiar código
syntax on
set number
set relativenumber
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent
set smartindent
set cursorline
set hlsearch
set incsearch
set showcmd
set scrolloff=2
set wrap
```

Essas opções melhoram a legibilidade, navegação e estrutura do código.

---

## Parte 5: Usando o Vim na Prática

---

### 🧭 Modos de Operação do Vim

O Vim funciona com diferentes **modos**, cada um com funções específicas:

- **Modo Normal**: padrão para navegação e execução de comandos (pressione `Esc`).
- **Modo de Inserção**: para digitar texto (`i`, `I`, `a`, `A`, `o`, `O`).
- **Modo Visual**: para selecionar texto (`v` para caractere, `V` para linha, `Ctrl+v` para bloco).
- **Modo de Comando**: usado para salvar, sair, buscar, etc. (pressione `:` no modo normal).

---

### ⌨️ Navegação Básica

Evite usar as setas! O Vim é mais rápido com os seguintes atalhos:

- `h` → esquerda  

- `j` → baixo  

- `k` → cima  

- `l` → direita  

  ---

#### 🧱 Movimento por palavras e linhas:
- `w` / `b`: próxima / anterior palavra  
- `0` / `$`: início / fim da linha  
- `gg` / `G`: topo / final do arquivo  

---

### ✍️ Inserção de Texto

- `i`: inserir antes do cursor  
- `I`: inserir no início da linha  
- `a`: inserir após o cursor  
- `A`: inserir no fim da linha  
- `o`: nova linha abaixo  
- `O`: nova linha acima  

---

### 💾 Salvar, Sair e Gerenciar Arquivos

- `:w` → salvar  
- `:q` → sair  
- `:wq` ou `:x` → salvar e sair  
- `:q!` → sair sem salvar  
- `:e!` → recarregar arquivo descartando mudanças  

---

### ✂️ Edição de Texto

- `dd` → deletar linha  
- `yy` → copiar linha  
- `p` → colar após  
- `P` → colar antes  

- `u` → desfazer  
- `Ctrl + r` → refazer  

---

### ⚡ Dica Rápida: Fluxo Comum

1. `vim arquivo.c` → abre o arquivo  
2. `i` → entra no modo de inserção  
3. Escreve o código  
4. `Esc` → volta ao modo normal  
5. `:wq` → salva e sai  

---

Com esses comandos, você já consegue escrever e editar código eficientemente dentro do Vim, sem tocar no mouse e sem depender de uma IDE.

Na próxima parte, veremos como compilar e executar esses códigos diretamente do terminal.

---

## Parte 6: Compilando Código no Terminal

---

Vamos ver como escrever, compilar e executar um programa usando o GCC.

📄 Exemplo em C: Hello World
Crie o arquivo:

```bash
vim hello.c
```
Digite o código:

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```
Salve e saia com `:wq.`

🛠️ Compile com GCC:
```bash
gcc hello.c -o hello
```
▶️ Execute:
```bash
./hello
```
🧠 Dica:
Use opções adicionais para melhorar a compilação:

```bash
gcc -Wall -g -O2 -std=c11 hello.c -o hello
```
---

## Parte 7: Lidando com Erros de Compilação

---

Durante o desenvolvimento, é comum encontrar **erros de compilação**. O compilador **GCC** fornece mensagens que indicam o tipo de erro e a linha onde ele ocorreu. Saber interpretá-las é essencial para depurar seu código com eficiência.

### 🐞 Exemplo de código com erro

Vamos criar um código em C com um erro de sintaxe proposital para entender como o GCC se comporta.

#### 1. Crie o arquivo:
```bash
vim erro.c
```
#### 2. Escreva o seguinte código com erro:
```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n"  // ← erro: parêntese não fechado
    return 0;
}
```

### 🛠️ Tentando compilar

No terminal, execute:
```bash
gcc erro.c -o erro
```

Você verá uma mensagem de erro como esta:
```bash
erro.c: In function ‘main’:
erro.c:4:5: error: expected ‘)’ before ‘return’
```

Essa mensagem diz que o compilador esperava um parêntese fechado ) antes da palavra return, na linha 4.

###  🧹 Corrigindo o erro no Vim

Abra o arquivo novamente:
```bash
vim erro.c
```

Corrija o código:
```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

Salve e saia (:wq)

###  ✔️ Compilando novamente

Agora compile novamente:
```bash
gcc erro.c -o erro
```

Se não houver mensagens de erro, o executável erro foi gerado com sucesso.

###  ▶️ Executando o programa corrigido
```bash
./erro
```

Saída:
```bash
Hello, World!
```
---
## Parte 8: Compilando Código Java com Vim e Terminal
---

Além de C, você também pode programar em **Java** usando o Vim e compilar diretamente pelo terminal usando o **JDK (Java Development Kit)**.

### ☕ Pré-requisitos

Certifique-se de que o **JDK** está instalado no seu sistema. Para verificar:

```bash
javac -version
```

Se não estiver instalado, use:

Ubuntu/Debian:
```bash
sudo apt install default-jdk
```
Fedora:
```bash
sudo dnf install java-1.8.0-openjdk-devel
```
Arch Linux:
```bash
sudo pacman -S jdk-openjdk
```
###  📝 Escrevendo um programa Java com Vim
1. Crie um arquivo .java:
```bash
vim Exemplo.java
```
2. Escreva o seguinte código:
```java
public class Exemplo {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
3. Salve e saia:

Pressione `Esc`

Digite `:wq` e pressione `Enter`

### 🛠️ Compilando o código Java

Para compilar o arquivo, use:
```bash
javac Exemplo.java
```

Esse comando cria um arquivo Exemplo.class, que é o bytecode do Java pronto para ser executado pela JVM.

### ▶️ Executando o programa

Depois de compilar, execute com:
```bash
java Exemplo
```
Saída esperada:
```bash
Hello, World!
```
### ⚠️ Dicas importantes

O nome da classe pública precisa ser o mesmo do arquivo (Exemplo.java → public class Exemplo).

O Java diferencia maiúsculas de minúsculas em nomes de arquivos e classes.

Se você receber o erro Could not find or load main class, verifique se compilou corretamente e está no mesmo diretório do .class.

### 📦 Compilando múltiplos arquivos Java

Se estiver trabalhando com vários arquivos, você pode compilá-los todos com:
```bash
javac *.java
```

E executar a classe principal normalmente:
```bash
java NomeDaClassePrincipal
```
---

## Considerações Finais

---

O Vim pode parecer intimidador no começo, mas com o tempo se torna uma das ferramentas mais poderosas no arsenal de qualquer programador.

Ao combiná-lo com o terminal e compiladores como o **GCC** (para C/C++) ou o **Javac** (para Java), você ganha:

- Total controle do ambiente de desenvolvimento
- Maior produtividade sem o uso do mouse
- Um setup leve, rápido e portátil
- Independência de IDEs pesadas

A prática leva à fluência. Com o tempo, você vai perceber que está codificando, salvando, compilando e executando seus programas com apenas alguns comandos rápidos — tudo sem sair do teclado.

Seja você um iniciante explorando o terminal ou um desenvolvedor avançado querendo mais agilidade, dominar o Vim é um investimento que vale a pena.

---

## Referências e Leituras Recomendadas

---

- 📘 [Documentação oficial do Vim](https://www.vim.org/docs.php)  
- 🔧 [GCC - GNU Compiler Collection](https://gcc.gnu.org/)  
- ☕ [Documentação oficial do Java SE](https://docs.oracle.com/javase/specs/)  
- 📓 [Vim Adventures](https://vim-adventures.com/) – jogo interativo para aprender Vim  
- 🎥 [The Primeagen - Vim & Neovim no YouTube](https://www.youtube.com/@ThePrimeagen)  
- 🔍 `:help` dentro do próprio Vim — a melhor forma de aprender comandos no contexto

---

> _“Aprenda Vim uma vez, e edite texto como um mestre para o resto da sua vida.”_

---
