# SOC Automation Labs: Detection, Analysis & Framework Mapping

Este repositório centraliza laboratórios práticos de Blue Team, focados em simulação de ataques, detecção via SIEM (Wazuh), monitoramento de integridade e resposta a incidentes, utilizando o framework MITRE ATT&CK como base estratégica.

## 🛠️ Stack Tecnológica
* **SIEM/XDR:** Wazuh (Manager & Agents)
* **Atacante:** Kali Linux (Hydra, Nmap, tcpdump)
* **Alvo:** Ubuntu Server 22.04 LTS
* **IDE:** VS Code

---

## 📂 Estrutura do Projeto

O repositório está organizado de forma a refletir um fluxo de trabalho SOC real:

### 1. [Lab 01, 03 & 04] - Detecção de Brute Force & Mapeamento MITRE
Este laboratório integrado demonstra o ciclo completo de uma tentativa de acesso não autorizado via SSH.
* **Simulação:** Uso do `Hydra` para ataque de dicionário contra o serviço SSH do Ubuntu.
* **Análise de Log:** Investigação profunda do arquivo `/var/log/auth.log` para identificar padrões de falha.
* **Detecção:** Configuração e visualização de alertas críticos no Dashboard do Wazuh (Regra 5712).
* **Inteligência:** Mapeamento do evento para a Tática de **Credential Access (TA0006)** e Técnica **Brute Force (T1110)** do MITRE ATT&CK.

### 2. [Lab 02] - File Integrity Monitoring (FIM)
Focado na detecção de mudanças em arquivos críticos do sistema após uma possível invasão.
* **Cenário:** Monitoramento em tempo real do diretório `/etc/ssh/`.
* **Ação:** Alteração manual do arquivo `sshd_config`.
* **Resultado:** Geração de alertas de "Integrity checksum changed", detalhando o usuário e o processo que realizou a modificação.

### 3. [Lab 05] - Network Analysis (Traffic Capture)
Análise de tráfego de rede para identificação de varreduras e exfiltração.
* **Ferramenta:** `tcpdump` no servidor alvo.
* **Objetivo:** Capturar e filtrar pacotes SYN (Port Scanning) vindos do Kali Linux para validar tentativas de reconhecimento.

---

## 🚀 Como Executar os Labs

### Pré-requisitos
1.  Ambiente de virtualização (VMware/VirtualBox).
2.  Instalação do Wazuh Server (All-in-one) no Ubuntu.
3.  Instalação do Wazuh Agent no servidor alvo.

### Passo a Passo Rápido (Lab 01)
1.  **No Ubuntu:** Certifique-se de que o log `/var/log/auth.log` está sendo monitorado pelo agente.
2.  **No Kali:** Execute:
    ```bash
    hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://[IP_DO_ALVO]
    ```
3.  **No Wazuh:** Acesse `Security Events` e filtre por "sshd" para visualizar o disparo do alerta de nível 10.

---

## 📊 Resultados e Evidências
Cada diretório de laboratório contém uma pasta `/screenshots` com:
* Alertas disparados no painel do Wazuh.
* Diferença entre logs de sucesso e falha via terminal.
* Tabelas de mitigação sugeridas para cada ameaça detectada.

---

## 🛡️ Mitigações Recomendadas
Como parte da análise estratégica, os labs incluem sugestões de hardening como:
* Implementação de autenticação via chaves SSH (Desabilitar senhas).
* Configuração de Fail2Ban para bloqueio automático de IPs.
* Restrição de acesso SSH via Firewall (UFW) apenas para IPs conhecidos.