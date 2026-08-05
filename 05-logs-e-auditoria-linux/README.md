# Laboratório 05 — Logs Linux e Auditoria de Eventos

## Cenário

Durante uma análise de segurança em um ambiente Linux, foi necessário investigar eventos registrados pelo sistema para identificar atividades administrativas, autenticações e possíveis comportamentos anormais.

Como analista, foram realizadas consultas aos registros do sistema com o objetivo de responder perguntas como:

- Quais usuários acessaram o sistema?
- Houve tentativas de autenticação falhas?
- Quais comandos foram executados com privilégios administrativos?
- Existem alertas ou eventos relevantes registrados?

O laboratório demonstra o processo de coleta e análise de evidências utilizando ferramentas nativas do Linux.

---

# Objetivo

Este laboratório tem como objetivo compreender:

- estrutura de logs do Linux;
- importância da auditoria de eventos;
- análise de autenticação;
- rastreamento de comandos administrativos;
- utilização do journal do systemd;
- identificação de sessões ativas.

---

# Ambiente

**Sistema operacional:**

```text
Ubuntu Linux
```

**Usuário administrador:**

```text
andrey-silva
```

**Ambiente utilizado:**

```text
Ubuntu Lab - Máquina Virtual
```

---

# Investigação Realizada

## 1. Identificação dos arquivos de log

Inicialmente foi realizada uma análise do diretório responsável pelo armazenamento de registros do sistema:

```bash
ls /var/log
```

Foram identificados arquivos importantes para auditoria:

- auth.log;
- syslog;
- kern.log;
- journal;
- wtmp.

Cada arquivo possui uma finalidade específica dentro do processo de investigação.

---

# 2. Análise de permissões do arquivo de autenticação

Arquivo analisado:

```text
/var/log/auth.log
```

Comando utilizado:

```bash
ls -l /var/log/auth.log
```

Resultado analisado:

```text
-rw-r----- 1 syslog adm
```

Interpretação:

| Usuário | Permissão |
|---|---|
| syslog (dono) | leitura e escrita |
| adm (grupo) | somente leitura |
| outros usuários | sem acesso |

A proteção desse arquivo é importante porque ele contém informações sensíveis relacionadas a:

- autenticação;
- usuários;
- uso do sudo;
- sessões do sistema.

O controle de acesso segue o princípio do menor privilégio.

---

# 3. Análise de eventos de autenticação

Comando utilizado:

```bash
sudo tail -20 /var/log/auth.log
```

Foram analisados eventos relacionados a:

- abertura de sessões;
- autenticação de usuários;
- elevação de privilégios;
- comandos administrativos.

Durante a análise foi identificado o usuário:

```text
andrey-silva
```

realizando ações administrativas utilizando sudo.

---

# 4. Investigação de falhas de autenticação

Foi realizada uma busca por tentativas de senha incorreta:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Resultado:

Nenhuma tentativa de autenticação com falha foi identificada durante o período analisado.

A utilização de filtros permite localizar rapidamente eventos específicos dentro de grandes volumes de logs.

---

# 5. Auditoria de privilégios administrativos

Comando utilizado:

```bash
sudo grep "sudo:" /var/log/auth.log | tail -10
```

A análise permitiu identificar:

- usuário responsável pela ação;
- elevação para root;
- comando executado;
- terminal utilizado.

Exemplo de evento identificado:

```text
USER=root
COMMAND=/usr/bin/grep
```

Esse tipo de registro permite rastrear ações administrativas realizadas no sistema.

---

# 6. Análise de eventos do systemd

Comando utilizado:

```bash
journalctl -xe
```

O journalctl permite consultar registros gerenciados pelo systemd.

Foram analisados:

- eventos recentes;
- inicialização de serviços;
- avisos do sistema;
- falhas operacionais.

A análise demonstrou a importância de diferenciar:

- erros operacionais;
- alertas esperados;
- possíveis incidentes de segurança.

---

# 7. Filtragem de alertas do sistema

Comando utilizado:

```bash
journalctl -p warning -n 20
```

O comando permitiu visualizar mensagens classificadas como warning ou superiores.

Foram encontrados eventos relacionados a:

- ambiente gráfico GNOME;
- VMware;
- serviços do sistema.

Após análise contextual, os eventos foram classificados como avisos operacionais e não como incidentes de segurança.

---

# 8. Análise de sessões ativas

Comando utilizado:

```bash
loginctl list-sessions
```

Foram identificadas sessões abertas do usuário:

```text
andrey-silva
```

A análise permitiu verificar:

- usuários conectados;
- UID;
- sessões existentes;
- ambiente utilizado.

Esse tipo de consulta é importante para identificar acessos desconhecidos em ambientes corporativos.

---

# Evidências

As evidências práticas estão armazenadas na pasta:

```text
imagens/
```

Contendo:

- estrutura de logs;
- permissões do auth.log;
- análise de autenticação;
- auditoria sudo;
- análise do journal;
- sessões de usuários.

---

# Conceitos de Segurança Aplicados

- Auditoria de sistemas Linux;
- Análise de logs;
- Autenticação e autorização;
- Controle de acesso;
- Princípio do menor privilégio;
- Rastreamento de atividades administrativas;
- Investigação de eventos;
- systemd journal;
- Monitoramento de sessões.

---

# Conclusão

O laboratório demonstrou como os registros do Linux podem ser utilizados como fonte de evidência durante uma análise de segurança.

Através dos logs foi possível identificar usuários, sessões, comandos administrativos e eventos do sistema.

A prática reforçou a importância da auditoria contínua, controle de privilégios e análise contextual dos eventos antes de classificar uma atividade como suspeita.
