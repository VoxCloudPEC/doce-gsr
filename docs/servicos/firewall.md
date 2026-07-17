---
icon: lucide/brick-wall-fire
---

# Firewall

## Ferramenta Escolhida
- [x] iptables
- [ ] nftables
- [ ] UFW
- [ ] firewalld
- [ ] Outra: _____________

---

### What (O quê?)
**1. Qual é o objetivo principal da implantação do firewall?**
Proteger a infraestrutura crítica de redes da VoxCloud (Matriz e Filial), garantindo o isolamento da Zona Desmilitarizada (DMZ), blindando os servidores internos através de uma política de bloqueio por padrão (Drop Default) e permitindo o tráfego seguro interunidades via túnel VPN.

**2. Quais serviços devem ser protegidos (SSH, HTTP, HTTPS, DNS, banco de dados etc.)?**
Devem ser protegidos o tráfego administrativo (SSH), o servidor Web institucional (HTTP/HTTPS na DMZ), o serviço de resolução de nomes (DNS), a base de dados do e-SUS PEC, o servidor de autenticação centralizada (AD DS) e todo o ecossistema de monitoramento (Zabbix, Grafana e Graylog).

**3. Quais portas e protocolos precisam ser liberados?**

*   **UDP 51820:** Comunicação da VPN WireGuard.
*   **TCP 22:** SSH (restrito à LAN e VPN).
*   **TCP/UDP 53:** Consultas DNS.
*   **TCP 80 e 443:** HTTP/HTTPS redirecionados para a DMZ.
*   **TCP 10050/10051:** Comunicação do Zabbix Agent/Server.
*   **UDP/TCP 12201:** Envio de logs para o Graylog.
*   **TCP 3000/9000:** Dashboards do Grafana e gerência via Portainer.

**4. Quais portas e protocolos devem ser bloqueados?**
Todas as portas e protocolos que não estão explicitamente liberados devem ser bloqueados pelas políticas padrão `DROP` nas chains `INPUT` e `FORWARD`. Adicionalmente, o tráfego originado da DMZ em direção à rede interna (LAN/VPN) é estritamente bloqueado.

**5. Qual ferramenta será utilizada: iptables, UFW ou firewalld?**
O `iptables`, por ser nativo, leve e permitir manipulação direta no kernel do Linux.

**6. Existem requisitos de registro (logs) para auditoria e monitoramento?**
Sim. Devido à natureza sensível dos dados médicos e às exigências de conformidade, os logs de firewall e tráfego suspeito serão encaminhados para o servidor centralizado Graylog.

---

### Why (Por quê?)
**1. Por que o firewall está sendo implantado?**
Para criar uma barreira perimetral robusta que separe a rede pública (internet) da rede interna, viabilizando o funcionamento da DMZ e do túnel VPN essenciais para o negócio da VoxCloud.

**2. Há alguma exigência de segurança, compliance ou auditoria?**
Sim. O sistema suporta o e-SUS PEC, lidando com dados sensíveis de saúde da população, o que exige conformidade rígida com as normativas da LGPD (Lei Geral de Proteção de Dados).

**3. Quais riscos se deseja mitigar com a solução?**
Busca-se mitigar acessos não autorizados à rede administrativa (LAN), vazamento de informações do banco de dados de pacientes, negação de serviço (DoS) e movimentação lateral caso o servidor Web (Nginx) na DMZ seja comprometido.

**4. Existe histórico de incidentes de segurança que justifique a implantação?**
Não há histórico por se tratar do projeto inicial da empresa no âmbito do Projeto Integrador, configurando uma abordagem proativa (Security by Design) e não reativa.

---

### Where (Onde?)
**1. Em qual ambiente o firewall será implantado (produção, homologação, desenvolvimento)?**
A implantação será efetuada no ambiente de Produção da VoxCloud.

**2. Em quais servidores ou estações Linux será aplicado?**
Nos servidores de borda/gateways (Matriz em Parnamirim/RN e Filial em Brasília/DF) rodando o sistema operacional Linux.

**3. O firewall protegerá acessos internos, externos ou ambos?**
Ambos. Ele filtra o tráfego da WAN (externo), roteia adequadamente os pacotes criptografados da VPN e isola logicamente a comunicação interna entre a LAN administrativa e a DMZ.

**4. Há segmentação de rede (DMZ, VLANs, sub-redes)?**
Sim. A infraestrutura conta com endereçamento IP isolado:
*   Rede LAN Matriz: `192.168.100.0/24`
*   Rede LAN Filial: `192.168.101.0/24`
*   Rede DMZ (Servidor Web): `192.168.200.0/30`

---

### When (Quando?)
**1. Qual a data prevista para implantação?**
A implantação foi programada para o ciclo de desenvolvimento do Projeto Integrador de 2026.

**2. Existe uma janela de manutenção definida?**
Janelas de manutenção estão atreladas aos períodos de inatividade definidos pela administração da VoxCloud e da clínica (geralmente finais de semana ou horários noturnos).

**3. Quando os testes de validação deverão ser realizados?**
Os testes de encaminhamento de pacotes, bloqueio inter-redes e fechamento do túnel WireGuard são realizados logo após o provisionamento das instâncias do firewall e imediatamente após cada atualização de regras.

**4. Há prazo para entrada em produção?**
O prazo coincide com a entrega final e defesa do Projeto Integrador no IFRN (4º período do curso).

---

### Who (Quem?)
**1. Quem será o responsável pela administração do firewall?**
Jean Matheus Prestes Fernandes (responsável técnico de segurança de borda).

**2. Quem aprovará as regras de acesso?**
A equipe diretiva da VoxCloud e, academicamente, o orientador do projeto, Credson Isaac Lopes dos Santos.

**3. Quem será impactado pelas mudanças?**
Os colaboradores da empresa (médicos, equipe de recepção, diretores) e a população que acessa o portal institucional na DMZ.

**4. Quem será responsável pelo monitoramento e análise dos logs?**
A equipe do Network Operations Center (NOC) e infraestrutura, através das plataformas Zabbix, Grafana e Graylog.

---

### How (Como?)
**1. Como as regras serão definidas e documentadas?**
As regras são estruturadas em um script shell documentado com comentários para cada bloco lógico (Ex: Políticas Padrão, Input, Forward, DMZ, NAT, WireGuard), facilitando a compreensão da equipe.

**2. Como será realizado o backup da configuração do firewall?**
As configurações ativas serão salvas utilizando o comando `iptables-save > /etc/iptables/rules.v4` e o arquivo do script original será versionado no repositório oficial do projeto no GitHub.

**3. Como serão realizados os testes de conectividade e segurança?**
Através de comandos de diagnóstico como `ping` (icmp-echo), testes de conexão via SSH, simulação de acessos ao Nginx pelo IP público e verificação de estabelecimento de túnel (`wg show`) entre as filiais.

**4. Como será feita a manutenção e atualização das regras?**
As modificações serão feitas inicialmente no ambiente de testes. Após a homologação, o script será atualizado no servidor em produção e as regras recarregadas sem a necessidade de reiniciar a máquina.

**5. Como será o processo de contingência em caso de bloqueio indevido?**
Em caso de perda de gerência (ex: bloqueio do SSH de administração), será utilizado o acesso ao console físico/virtual do servidor para executar o *flush* das regras (`iptables -F` e `iptables -X`) ou restaurar o arquivo de backup funcional mais recente.

**6. Como será garantida a persistência das regras após reinicializações?**
Utilizando o utilitário `iptables-persistent`. As regras funcionais serão gravadas no diretório `/etc/iptables/rules.v4`, garantindo que o sistema operacional as restaure automaticamente em qualquer evento de reinicialização (reboot).

####Firewall - Matriz
```
# ─── LIMPAR REGRAS ──────────────────────────────────────────
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# ─── POLÍTICAS PADRÃO ───────────────────────────────────────
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# ─── INPUT ──────────────────────────────────────────────────
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT

# SSH só da LAN e VPN
iptables -A INPUT -i enp0s8 -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -i wg0 -p tcp --dport 22 -j ACCEPT

# Ping interno
iptables -A INPUT -i enp0s8 -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -i wg0 -p icmp --icmp-type echo-request -j ACCEPT

# DNS
iptables -A INPUT -i enp0s8 -p udp --dport 53 -j ACCEPT
iptables -A INPUT -i enp0s8 -p tcp --dport 53 -j ACCEPT

# WireGuard
iptables -A INPUT -i enp0s3 -p udp --dport 51820 -j ACCEPT

# Zabbix Agent (aceita do servidor 192.168.100.2)
iptables -A INPUT -i enp0s8 -s 192.168.100.2 -p tcp --dport 10050 -j ACCEPT

# ─── FORWARD: CONEXÕES ESTABELECIDAS ────────────────────────
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

# ─── FORWARD: LAN → WAN ─────────────────────────────────────
iptables -A FORWARD -i enp0s8 -o enp0s3 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

# ─── FORWARD: LAN → DMZ (site) ──────────────────────────────
iptables -A FORWARD -i enp0s8 -o enp0s9 -d 192.168.200.2 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -i enp0s8 -o enp0s9 -d 192.168.200.2 -p tcp --dport 443 -j ACCEPT
iptables -A FORWARD -i enp0s8 -o enp0s9 -d 192.168.200.2 -p tcp --dport 10050 -j ACCEPT

# ─── FORWARD: VPN → DMZ (site) ──────────────────────────────
iptables -A FORWARD -i wg0 -o enp0s9 -d 192.168.200.2 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -i wg0 -o enp0s9 -d 192.168.200.2 -p tcp --dport 443 -j ACCEPT

# ─── FORWARD: DMZ → Zabbix ──────────────────────────────────
iptables -A FORWARD -i enp0s9 -o enp0s8 -d 192.168.100.2 -p tcp --dport 10050 -j ACCEPT
iptables -A FORWARD -i enp0s9 -o enp0s8 -d 192.168.100.2 -p tcp --dport 10051 -j ACCEPT

# ─── FORWARD: DMZ → Graylog ─────────────────────────────────
iptables -A FORWARD -i enp0s9 -o enp0s8 -d 192.168.100.2 -p tcp --dport 12201 -j ACCEPT
iptables -A FORWARD -i enp0s9 -o enp0s8 -d 192.168.100.2 -p udp --dport 12201 -j ACCEPT

# ─── FORWARD: DMZ → WAN (atualizações) ──────────────────────
iptables -A FORWARD -i enp0s9 -o enp0s3 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

# ─── FORWARD: LAN → Zabbix/Graylog/Grafana ──────────────────
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p tcp --dport 10050 -j ACCEPT
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p tcp --dport 10051 -j ACCEPT
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p tcp --dport 3000 -j ACCEPT
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p tcp --dport 9000 -j ACCEPT
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p tcp --dport 12201 -j ACCEPT
iptables -A FORWARD -i enp0s8 -d 192.168.100.2 -p udp --dport 12201 -j ACCEPT

# ─── FORWARD: VPN → Zabbix/Graylog/Grafana ──────────────────
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 10050 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 10051 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 3000 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 9000 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 12201 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p udp --dport 12201 -j ACCEPT

# ─── FORWARD: DMZ → LAN/VPN bloqueado ───────────────────────
iptables -A FORWARD -i enp0s9 -o enp0s8 -j DROP
iptables -A FORWARD -i enp0s9 -o wg0 -j DROP

# ─── FORWARD: Matriz ↔ Filial via VPN ───────────────────────
iptables -A FORWARD -i wg0 -o enp0s8 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -i enp0s8 -o wg0 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

# ─── NAT ────────────────────────────────────────────────────
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# ─── PROTEÇÃO ───────────────────────────────────────────────
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP
iptables -A INPUT -i enp0s3 -p tcp --dport 22 -m limit --limit 3/min -j ACCEPT

# ─── SALVAR ─────────────────────────────────────────────────
iptables-save > /etc/iptables/rules.v4
```

####Firewall - Filial

```
# ─── LIMPAR REGRAS ──────────────────────────────────────────
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# ─── POLÍTICAS PADRÃO ───────────────────────────────────────
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# ─── INPUT ──────────────────────────────────────────────────
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT

# SSH só da LAN e VPN
iptables -A INPUT -i enp0s8 -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -i wg0 -p tcp --dport 22 -j ACCEPT

# Ping interno
iptables -A INPUT -i enp0s8 -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -i wg0 -p icmp --icmp-type echo-request -j ACCEPT

# WireGuard
iptables -A INPUT -i enp0s3 -p udp --dport 51820 -j ACCEPT

# Zabbix Agent (aceita do servidor via VPN)
iptables -A INPUT -i wg0 -s 192.168.100.2 -p tcp --dport 10050 -j ACCEPT
iptables -A INPUT -i enp0s8 -s 192.168.101.2 -p tcp --dport 10050 -j ACCEPT

# ─── FORWARD: CONEXÕES ESTABELECIDAS ────────────────────────
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

# ─── FORWARD: LAN → WAN ─────────────────────────────────────
iptables -A FORWARD -i enp0s8 -o enp0s3 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

# ─── FORWARD: LAN Filial → Site da Matriz via VPN ───────────
iptables -A FORWARD -i enp0s8 -o wg0 -d 192.168.200.2 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -i enp0s8 -o wg0 -d 192.168.200.2 -p tcp --dport 443 -j ACCEPT

# ─── FORWARD: VPN → Zabbix/Graylog/Grafana ──────────────────
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 10050 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 10051 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 3000 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 9000 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p tcp --dport 12201 -j ACCEPT
iptables -A FORWARD -i wg0 -d 192.168.100.2 -p udp --dport 12201 -j ACCEPT

# ─── FORWARD: Filial ↔ Matriz via VPN ───────────────────────
iptables -A FORWARD -i wg0 -o enp0s8 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -i enp0s8 -o wg0 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

# ─── NAT ────────────────────────────────────────────────────
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# ─── PROTEÇÃO ───────────────────────────────────────────────
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP
iptables -A INPUT -i enp0s3 -p tcp --dport 22 -m limit --limit 3/min -j ACCEPT

# ─── SALVAR ─────────────────────────────────────────────────
iptables-save > /etc/iptables/rules.v4
```

