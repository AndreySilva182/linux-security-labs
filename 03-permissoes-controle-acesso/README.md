# Laboratório 03 — Permissões e Controle de Acesso Linux

## Objetivo

Compreender o funcionamento das permissões Linux, controle de acesso baseado em usuários e grupos, utilização dos comandos `chmod` e `chown`, além da aplicação do princípio do menor privilégio.

---

# Ambiente

**Sistema operacional:**

Ubuntu Linux

**Usuário administrador:**

andrey-silva

**Usuário utilizado para testes:**

joao

**Arquivo utilizado nos testes:**

```bash
seguranca.txt
```

---

# 1. Análise inicial das permissões

Foi realizada a análise das permissões iniciais do arquivo utilizando o comando:

```bash
ls -l seguranca.txt
```

Permissão inicial encontrada:

```text
-rw-rw-r--
```

Interpretação:

| Usuário | Permissão |
|---|---|
| Proprietário | Leitura e escrita |
| Grupo | Leitura e escrita |
| Outros usuários | Somente leitura |

Neste cenário, usuários pertencentes ao grupo poderiam modificar o arquivo.

---

# 2. Aplicação do princípio do menor privilégio

Foi aplicada uma restrição de acesso utilizando:

```bash
chmod 600 seguranca.txt
```

Resultado obtido:

```text
-rw-------
```

Após a alteração:

| Usuário | Acesso |
|---|---|
| Proprietário | Leitura e escrita |
| Grupo | Sem permissão |
| Outros usuários | Sem permissão |

A configuração garante que somente o proprietário do arquivo consiga realizar alterações.

Essa prática segue o princípio de segurança conhecido como **menor privilégio**, onde cada usuário recebe apenas o acesso necessário para executar sua função.

---

# 3. Teste de acesso com usuário comum

Foi realizado o teste utilizando o usuário João:

```bash
su - joao
```

Tentativa de leitura do arquivo:

```bash
cat /home/andrey-silva/laboratorio/permissoes/seguranca.txt
```

Resultado:

```text
Permission denied
```

Análise:

O Linux bloqueou o acesso porque o usuário João não possuía permissão para acessar o arquivo.

Esse comportamento demonstra o funcionamento do controle de acesso baseado em permissões.

---

# 4. Controle de acesso utilizando grupos

Para representar um cenário corporativo, foi criado um grupo funcional:

```text
financeiro
```

O usuário João foi adicionado ao grupo:

```bash
sudo usermod -aG financeiro joao
```

Validação do grupo:

```bash
groups joao
```

Resultado:

```text
joao : joao financeiro
```

Agora o usuário João pertence ao grupo financeiro.

---

# 5. Alteração do grupo do arquivo

O arquivo foi associado ao grupo financeiro utilizando:

```bash
sudo chown andrey-silva:financeiro seguranca.txt
```

Depois foram definidas as permissões:

```bash
chmod 640 seguranca.txt
```

Resultado final:

```text
-rw-r----- 1 andrey-silva financeiro seguranca.txt
```

Interpretação:

| Usuário | Acesso |
|---|---|
| andrey-silva | Ler e modificar |
| Grupo financeiro | Somente leitura |
| Outros usuários | Sem acesso |

Essa configuração permite compartilhar informações com um grupo específico sem liberar acesso geral aos demais usuários.

---

# 6. Validação das permissões

Foi realizada a validação final utilizando:

```bash
ls -l seguranca.txt
```

O resultado confirmou que:

- O proprietário mantém controle total sobre o arquivo;
- O grupo financeiro possui apenas permissão de leitura;
- Outros usuários não possuem acesso.

---

# Evidências

Os testes realizados estão documentados na pasta:

```
imagens/
```

Contendo registros dos seguintes procedimentos:

- Alteração de permissões utilizando `chmod`;
- Bloqueio de acesso do usuário João;
- Criação e utilização do grupo financeiro;
- Alteração de proprietário e grupo do arquivo;
- Validação final das permissões.

---

# Conceitos de Segurança Aplicados

- Controle de acesso Linux;
- Usuários e grupos;
- Permissões de arquivos (`rwx`);
- Comando `chmod`;
- Comando `chown`;
- Gerenciamento de privilégios;
- Princípio do menor privilégio;
- Separação de responsabilidades;
- Controle de acesso baseado em grupos.

---

# Conclusão

O laboratório demonstrou como o Linux utiliza usuários, grupos e permissões para proteger arquivos e controlar acessos.

A aplicação correta das permissões evita alterações indevidas, reduz riscos de exposição de informações e segue boas práticas de Segurança da Informação.

O gerenciamento adequado de privilégios é fundamental para ambientes corporativos, garantindo que cada usuário tenha somente o acesso necessário para executar suas atividades.
