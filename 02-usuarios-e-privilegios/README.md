# Laboratório 02 — Usuários e Privilégios Linux

## Objetivo

Este laboratório tem como objetivo compreender o gerenciamento de usuários no Linux, criação de contas, controle de privilégios administrativos e aplicação do princípio do menor privilégio.

---

## Ambiente

Sistema operacional: Ubuntu Linux

Usuário administrador:
andrey-silva

Usuário criado para testes:
joao

---

## 1. Criação do usuário

Comando utilizado:

```bash
sudo useradd -m joao

A opção -m cria automaticamente o diretório pessoal do usuário:

/home/joao
2. Validação da criação do usuário

Comando utilizado:

id joao

Resultado obtido:

uid=1001(joao)
gid=1001(joao)
groups=1001(joao)

Análise:

O usuário João foi criado corretamente e possui seu próprio UID e GID.

Ele não pertence ao grupo administrativo sudo.

3. Comparação entre usuários
Usuário administrador

Comando:

id andrey-silva

Resultado:

groups=1000(andrey-silva),27(sudo)

O usuário Andrey pertence ao grupo sudo, permitindo executar comandos administrativos temporariamente.

Usuário comum

Comando:

id joao

Resultado:

groups=1001(joao)

O usuário João possui apenas seu grupo pessoal.

4. Teste de privilégio administrativo

Foi realizado o teste utilizando:

sudo whoami
Como usuário administrador:

Resultado:

root
Como usuário João:

Resultado:

sudo: joao is not allowed to run sudo

Análise:

O Linux bloqueou a tentativa de elevação de privilégio porque João não possui autorização administrativa.

5. Teste de isolamento entre usuários

Foi testado o acesso ao diretório administrativo:

cd /root

Resultado:

Permission denied

Também foi testado o acesso ao diretório de outro usuário:

cd /home/andrey-silva

Resultado:

Permission denied

Isso demonstra o isolamento entre usuários no Linux.

Conceitos de Segurança Aplicados
Usuários e grupos Linux;
UID e GID;
Autenticação;
Autorização;
Controle de acesso;
sudo;
Princípio do menor privilégio;
Separação de responsabilidades;
Isolamento de usuários.
Conclusão

O laboratório demonstrou como o Linux controla permissões através de usuários e grupos.

A utilização de contas individuais e privilégios administrativos temporários aumenta a segurança do ambiente, melhora a rastreabilidade das ações e reduz riscos de alterações indevidas.
