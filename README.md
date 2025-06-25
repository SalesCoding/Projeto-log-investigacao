# 🕵️ Projeto: Investigando um Acesso Malicioso no Linux

## 💡 Descrição
Esse projeto simula um ambiente Linux comprometido, onde um usuário malicioso “*atacante*” foi criado de forma suspeita. A partir de logs do sistema e comandos de investigação, foi realizada uma análise para identificar as atividades desse usuário e entender o que foi feito. Essa é uma atividade prática essencial para um Analista SOC/NOC.

---

## Cenário Simulado

- Um novo usuário chamado `atacante` é criado com shell bash.
- O usuário tenta utilizar `su` e `sudo`, sem sucesso.
- Horário do sistema é alterado para tentar mascarar a atividade.
- Tentativas de backdoor são realizadas.

---

## Ferramentas e Comandos Usados

### Análise de Logs:
- `/var/log/auth.log`
- `journalctl -xe`

### ⚙omandos Utilizados:
```bash
# Verificação de logs de login e sudo
sudo cat /var/log/auth.log | grep atacante
sudo journalctl | grep atacante

# Conferindo shell e diretórios de usuários
cat /etc/passwd | grep '/home'

# Verificando arquivos suspeitos
sudo ls -la /home/atacante
sudo find /home/atacante -name authorized_keys

# Tentativas de conexão
sudo grep 'Accepted publickey' /var/log/auth.log
sudo grep 'Failed publickey' /var/log/auth.log

# Alterando o horário do sistema
sudo date -s "2022-01-01 01:01:01"
```

---

## Resultados da Investigação

- O usuário `atacante` foi criado manualmente:
  - Logs mostram `groupadd`, `useradd` e `passwd` para esse usuário.
- Foram feitas **múltiplas tentativas de `su`** para o usuário atacante sem sucesso.
- Não foi identificado acesso via chave pública (`authorized_keys` ausente).
- Tentativas de criar um script com backdoor em `/tmp/startup.sh`, mas **sem privilégios para executá-lo**.
- Horário do sistema foi alterado manualmente com `date -s`, indicando tentativa de mascaramento.

---

## Habilidades Demonstradas

- Leitura e interpretação de logs do Linux
- Investigação de criação de usuários suspeitos
- Detecção de backdoors simples
- Manipulação e correlação de eventos em `/var/log` e `journalctl`
- Conhecimento básico de técnicas de ofuscação (data/hora)

---

## Links Importantes

- [Documentação do journalctl](https://man7.org/linux/man-pages/man1/journalctl.1.html)
- [Permissões de usuários e sudo no Linux](https://wiki.debian.org/sudo)

---

## Próximos Passos
- Automatizar a coleta de logs suspeitos com scripts em Bash
- Simular uma invasão via SSH com `fail2ban`
- Criar dashboards com Wazuh para detectar cenários similares (futuramente)

---

> Projeto criado como parte da trilha de estudos para vagas SOC/NOC com foco em Blue Team. 
> Investigador: Felipe (@PhantomHat)
