# Laboratório 04 — Processos Linux e Gerenciamento de Serviços

## Cenário

Durante uma análise em um ambiente Linux, foi identificado que um sistema apresentava necessidade de investigação dos processos e serviços em execução.

Como administrador do sistema, foi realizada uma análise para identificar:

- processos ativos;
- consumo de recursos;
- serviços em execução;
- controle de inicialização;
- registros de eventos do sistema.

O objetivo foi compreender como o Linux gerencia processos e serviços, aplicando conceitos básicos de administração e Segurança da Informação.

---

# Objetivo

Este laboratório tem como objetivo compreender:

- funcionamento dos processos Linux;
- identificação de processos através de PID;
- monitoramento de recursos;
- gerenciamento de serviços utilizando systemd;
- análise de logs;
- aplicação de boas práticas de administração segura.

---

# Ambiente

**Sistema operacional:**

Ubuntu Linux

**Usuário administrador:**

```bash
andrey-silva
```

**Ambiente utilizado:**

```text
Ubuntu Lab - Máquina Virtual
```

---

# Investigação Realizada

## 1. Reconhecimento do Sistema

Inicialmente foram coletadas informações básicas do ambiente utilizando:

```bash
whoami
hostname
uptime
```

Os comandos permitiram identificar:

- usuário atualmente conectado;
- nome do equipamento;
- tempo de atividade e carga do sistema.

Essas informações são importantes para validar o ambiente antes de iniciar uma investigação.

---

# 2. Análise de Processos Linux

## Listagem inicial de processos

Comando utilizado:

```bash
ps
```

O comando apresentou os processos associados ao terminal atual.

Foram identificadas informações como:

- PID (Process Identifier);
- terminal associado;
- tempo de processamento;
- comando executado.

O PID permite identificar individualmente cada processo em execução no sistema.

---

## Análise completa dos processos

Comando utilizado:

```bash
ps aux
```

O comando permitiu visualizar informações detalhadas dos processos:

- usuário responsável;
- PID;
- consumo de CPU;
- consumo de memória;
- comando executado.

Essa análise é utilizada para identificar processos inesperados ou consumo anormal de recursos.

---

# 3. Monitoramento em Tempo Real

Comando utilizado:

```bash
top
```

O comando permitiu acompanhar os processos em execução em tempo real.

Durante a análise, o processo com maior consumo identificado foi:

```text
gnome-shell
```

Análise:

O processo foi considerado esperado, pois está relacionado ao ambiente gráfico GNOME utilizado na máquina Ubuntu Desktop.

Em ambientes corporativos, a análise deve considerar o contexto do servidor antes de classificar um processo como suspeito.

---

# 4. Controle de Processos

Foi criado um processo de teste utilizando:

```bash
sleep 300 &
```

Após a identificação do PID através do comando:

```bash
ps
```

O processo foi encerrado utilizando:

```bash
kill PID
```

Exemplo:

```bash
kill 4729
```

Análise:

O comando `kill` envia sinais aos processos permitindo encerramento controlado.

A utilização do PID reduz o risco de finalizar processos incorretos.

---

# 5. Gerenciamento de Serviços Linux

O gerenciamento de serviços foi realizado utilizando o systemd através do comando:

```bash
systemctl
```

Inicialmente foram listados os serviços disponíveis:

```bash
systemctl --type=service
```

Foram analisados serviços ativos do sistema.

---

# 6. Análise do Serviço Cron

Serviço analisado:

```text
cron.service
```

Comando utilizado:

```bash
systemctl status cron
```

Foram analisadas informações como:

- status do serviço;
- PID principal;
- usuário de execução;
- registros recentes.

O serviço cron é responsável pela execução de tarefas automatizadas no Linux.

Exemplos:

- backups;
- scripts de manutenção;
- rotinas agendadas.

---

# 7. Controle de Serviços

Foi utilizado o serviço:

```text
cups.service
```

Inicialmente o serviço estava em execução.

Para interromper temporariamente o serviço foi utilizado:

```bash
sudo systemctl stop cups
```

Resultado:

```text
Active: inactive (dead)
```

O serviço deixou de executar, porém continuava configurado para iniciar automaticamente.

---

## Desabilitando inicialização automática

Comando utilizado:

```bash
sudo systemctl disable cups
```

Resultado:

```text
Loaded: loaded (...; disabled)
```

O serviço deixou de iniciar automaticamente durante a inicialização do sistema.

Essa prática é utilizada em processos de hardening para reduzir serviços desnecessários ativos no ambiente.

---

# 8. Análise de Logs

Para consultar eventos relacionados ao serviço foi utilizado:

```bash
journalctl -u cups
```

Os registros permitiram identificar:

- inicialização do serviço;
- parada controlada;
- eventos registrados pelo systemd.

A análise de logs é fundamental para auditoria e investigação de incidentes.

---

# Evidências

As evidências práticas estão armazenadas na pasta:

```text
imagens/
```

Contendo registros de:

- reconhecimento do sistema;
- análise de processos;
- monitoramento em tempo real;
- controle de processos;
- gerenciamento de serviços;
- análise de logs.

---

# Conceitos de Segurança Aplicados

- Processos Linux;
- PID (Process Identifier);
- Monitoramento de recursos;
- Usuários e privilégios;
- Princípio do menor privilégio;
- systemd;
- systemctl;
- Gerenciamento de serviços;
- Controle de inicialização;
- Auditoria através de logs;
- Investigação de comportamento anômalo.

---

# Conclusão

O laboratório demonstrou como o Linux permite monitorar, analisar e controlar processos e serviços de forma estruturada.

A identificação correta de processos, o gerenciamento seguro de serviços e a análise de logs são conhecimentos fundamentais para administração de sistemas e Segurança da Informação.

A prática reforçou a importância de investigar o comportamento do sistema antes de realizar alterações, preservando informações e reduzindo riscos operacionais.
