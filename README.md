# Introdução ao Docker no Windows com WSL

> 🎯 **Objetivo deste repositório**
> Ensinar **apenas os conceitos básicos e essenciais de Docker**, o suficiente para você:
>
> * entender o que está acontecendo
> * saber identificar problemas
> * conseguir evoluir sozinho
>
> Todo o resto (comandos específicos, variações, casos raros) **não precisa ser decorado** — isso será resolvido com **boas perguntas para IA**.

Este repositório tem como objetivo **ensinar Docker do zero**, usando **Windows + WSL + Ubuntu**, de forma prática e próxima da realidade de produção.

A ideia é montar um ambiente de desenvolvimento moderno, estável e profissional, evitando o famoso problema do *"funciona na minha máquina"*.

---

## Visão geral da arquitetura

Aqui o foco é entender **o mínimo necessário** para trabalhar com Docker no dia a dia.

Você **não precisa memorizar comandos**, flags ou caminhos específicos. O que você precisa saber é:

* o que é Windows
* o que é WSL
* o que é Linux
* onde o Docker roda
* quem conversa com quem

O resto pode (e deve) ser perguntado para IA.

> **Nota importante sobre aprendizado**
> Neste repositório, o foco **não é decorar comandos** ou memorizar passo a passo. Na prática profissional, desenvolvedores usam **documentação oficial** e **IA (como ChatGPT)** o tempo todo.
>
> O objetivo aqui é **entender o que está acontecendo**, saber **onde procurar** e **como validar** se algo está funcionando corretamente.

Neste setup, teremos:

* **Windows** como sistema operacional principal
* **WSL 2 (Windows Subsystem for Linux)** rodando um **Ubuntu**
* **Docker Engine** instalado dentro do Linux (WSL)
* **VS Code** integrado ao WSL para desenvolvimento

> Importante: apesar de estar rodando dentro do Windows, o Docker estará operando em um **ambiente Linux real**, como acontece em servidores.

---

## 1. O que é WSL e por que usar

O **WSL (Windows Subsystem for Linux)** permite rodar um ambiente Linux completo dentro do Windows, sem necessidade de máquina virtual tradicional.

### Por que usar WSL para Docker?

* A maioria dos servidores roda Linux
* Docker foi criado para Linux
* Melhor compatibilidade com ferramentas de backend e infra
* Menos problemas de performance e permissões

Neste projeto, **todo o Docker roda dentro do Linux (Ubuntu)**, não diretamente no Windows.

---

## 2. Instalando o WSL

> Pré-requisito: Windows 10 ou 11 atualizado

Instale o WSL com:

```bash
wsl --install
```

Esse comando irá:

* Ativar o WSL
* Instalar o WSL 2
* Instalar uma distro Linux (geralmente Ubuntu)

Após a instalação, reinicie o computador se solicitado.

Para verificar se está tudo certo:

```bash
wsl --list --verbose
```

Você deve ver algo como:

```text
Ubuntu    Running    2
```

---

## 3. Instalando e integrando o VS Code com o WSL

O **VS Code** será usado como editor de código, mas executando **dentro do Linux**, não no Windows.

### Passos

1. Instale o VS Code no Windows
2. No VS Code, instale a extensão:

   * **WSL** (Remote - WSL)

Essa extensão permite:

* Abrir projetos que estão no Linux
* Editar arquivos como `Dockerfile`, `docker-compose.yml`
* Usar terminal integrado já dentro do WSL

> Mais adiante, o VS Code será usado para criar e manter os arquivos de configuração do Docker.

Para abrir o VS Code direto no WSL:

```bash
code .
```

---

## 4. Instalando o Docker Engine no Ubuntu (WSL)

Aqui vamos seguir **exatamente a documentação oficial**.

🔗 Link oficial:
[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

> ⚠️ Importante: **não decore os comandos abaixo**.
> O objetivo é entender o fluxo:
>
> 1. preparar o sistema
> 2. adicionar o repositório
> 3. instalar o Docker Engine

### 4.1 Preparação do sistema (conceito)

Nesta etapa você aprende que:

* Linux usa um gerenciador de pacotes (`apt`)
* software vem de repositórios
* segurança é garantida por chaves GPG

Os comandos específicos **não precisam ser memorizados**.

---

### 4.2 Adicionando a chave GPG oficial do Docker

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Essa chave garante que os pacotes baixados são **oficiais e confiáveis**.

---

### 4.3 Adicionando o repositório do Docker

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Atualize a lista de pacotes novamente:

```bash
sudo apt update
```

Agora o sistema já conhece os pacotes oficiais do Docker.

---

### 4.4 Instalando o Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Isso instala:

* Docker Engine
* CLI do Docker
* Container runtime
* Docker Compose (plugin moderno)

---

## 5. Pós-instalação do Docker (Passo essencial)

Esta etapa existe para resolver **um único problema**:

> Por padrão, o Docker só funciona com `sudo`.

O conceito importante aqui é:

* usuários
* grupos
* permissões

Você **não precisa decorar** os comandos de pós-instalação.

O que você precisa saber é:

* existe um grupo chamado `docker`
* seu usuário precisa fazer parte dele
* isso permite usar Docker sem `sudo`

🔗 Documentação oficial:
[https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

---

### A solução: grupo `docker`

Crie (ou garanta) o grupo docker:

```bash
sudo groupadd docker
```

Adicione seu usuário ao grupo:

```bash
sudo usermod -aG docker $USER
```

Depois disso:

* **Feche e reabra o terminal**
* Ou reinicie o WSL

---

### Testando a instalação

Execute:

```bash
docker run hello-world
```

Se aparecer a mensagem de sucesso **sem usar sudo**, está tudo funcionando corretamente.

---

## 6. O que você aprendeu até aqui

Até aqui, você aprendeu **o essencial**:

* Docker roda em Linux
* No Windows, usamos WSL para isso
* Docker tem um daemon que roda como root
* Você conversa com esse daemon via comandos
* Permissões importam

Você **não decorou comandos** — e isso é intencional.

---

## 7. Usando IA a seu favor (parte mais importante)

Na prática profissional, ninguém decora Docker.

O que bons devs fazem é **saber perguntar bem**.

### Exemplos de bons prompts

```text
Explique com palavras simples o que este comando Docker faz:
<cole o comando aqui>
```

```text
Estou usando Docker no WSL. Esse erro significa o quê?
<cole o erro aqui>
```

```text
Qual é a forma correta e atual de instalar Docker Engine no Ubuntu 22.04?
```

```text
Preciso de um Dockerfile simples para um projeto Node.js.
Explique cada linha.
```

### Regra de ouro

> Use a IA para **lembrar comandos**.
> Use você mesmo para **pensar e decidir**.

Esse é o equilíbrio correto.

Mais importante do que decorar comandos, você aprendeu a:

* Ler e seguir **documentação oficial**
* Entender **por que** cada etapa existe
* Validar se algo realmente está funcionando
* Usar Linux, Docker e WSL de forma consciente
* Não depender de memória, e sim de **raciocínio + boas fontes**

Esse é exatamente o modelo de trabalho no mundo real.

Até este ponto, você já configurou:

* WSL com Linux real
* Ambiente de desenvolvimento integrado ao VS Code
* Docker Engine rodando no Linux
* Permissões corretas para uso profissional do Docker

Esse é o **mesmo modelo usado em muitos ambientes reais de desenvolvimento e servidores**.

---

## Próximos passos (roadmap)

* Conceitos básicos: imagem vs container
* Comandos essenciais do Docker
* Dockerfile na prática
* Docker Compose
* Docker aplicado a projetos reais (Node, Laravel, etc.)

---

## Observação final

Este repositório **não foca apenas em rodar comandos**, mas em **entender o que está acontecendo por baixo**, para evitar erros comuns e ganhar autonomia em ambientes reais.
