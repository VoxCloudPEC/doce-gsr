---
icon: lucide/chart-line
---

# Zabbix

## Registro de Instalação, Configuração e Uso
A instalação do servidor Zabbix foi realizada de forma conteinerizada, utilizando o gerenciador **Portainer** na rede interna da Matriz (IP `192.168.100.2`). A configuração incluiu a implantação de agentes (Zabbix Agent) em todos os hosts críticos da infraestrutura: Windows Server (AD DS), instâncias Linux (Firewalls Matriz e Filial) e o servidor Nginx hospedado na DMZ.

O uso da ferramenta permite a visualização centralizada do status da rede, complementada pelos dashboards visuais do **Grafana** e pela centralização de logs do **Graylog**. 

<img width="1198" height="259" alt="image" src="https://github.com/user-attachments/assets/3894505b-3cd6-4e1b-9863-c9086ad4189d" />

---

### What (O quê?)

**1. Qual é o objetivo da implantação do sistema de monitoramento?**
Garantir a disponibilidade, estabilidade e desempenho da infraestrutura que sustenta o Prontuário Eletrônico do Cidadão (PEC), evitando falhas de comunicação que geram ociosidade e filas nas unidades de saúde.

**2. Qual ferramenta será adotada?**

- [x] Zabbix
- [ ] Nagios
- [ ] Icinga
- [ ] Outra: __________
- [ ] A definir após análise técnica

**3. Quais ativos serão monitorados?**

- [x] Servidores Linux (Firewalls, DMZ, Docker)
- [x] Servidores Windows (Active Directory)
- [x] Banco de dados (PEC)
- [x] Equipamentos de rede (Switches e Roteadores)
- [x] Aplicações (Servidor Web Nginx, e-SUS)
- [ ] Serviços em nuvem

**4. Quais métricas deverão ser acompanhadas?**

- [x] CPU
- [x] Memória
- [x] Disco
- [x] Rede
- [x] Disponibilidade (Uptime)
- [x] Logs (Em conjunto com o Graylog)
- [ ] Outras

**5. Haverá necessidade de dashboards personalizados?**
Sim. Os dados coletados pelo Zabbix alimentarão os dashboards visuais desenvolvidos no Grafana para facilitar a análise de métricas de rede e apresentar resultados claros à gestão.

**6. Será necessário envio de alertas automáticos?**
Sim, fundamental para identificar latência na rede ou quedas do túnel VPN antes que impactem o atendimento médico.

---

### Why (Por quê?)

**1. Por que a organização deseja implantar um sistema de monitoramento?**
Para entregar uma infraestrutura "invisível, porém infalível", que mantenha o ecossistema conectado funcionando 24/7 sem interrupções nos serviços de saúde pública.

**2. Quais problemas atuais a solução pretende resolver?**
A lentidão na identificação de gargalos de rede, ociosidade do sistema de prontuários por quedas de link e perda de sincronia do banco de dados com o Ministério da Saúde.

**3. Existe dificuldade em identificar falhas e indisponibilidades?**
Por possuir servidores fisicamente distribuídos entre a Matriz (Parnamirim/RN) e a Filial (Brasília/DF), a identificação manual de falhas seria inviável sem uma centralização.

**4. Há necessidade de monitoramento proativo?**
Sim, atuar em um ambiente hospitalar/clínico requer a mitigação do problema antes que o serviço saia do ar.

**5. Existem exigências de auditoria ou conformidade?**
Sim. A operação com dados de saúde requer total conformidade com as diretrizes da LGPD, o que obriga o rastreamento, monitoramento de acessos e auditoria constante.

**6. Quais benefícios são esperados após a implantação?**
Alta disponibilidade (meta de 99.9% de uptime), segurança proativa e redução do tempo de resposta (SLA) para incidentes.

---

### Where (Onde?)

**1. Onde a solução será instalada?**

- [x] Ambiente local *(No servidor Docker/Portainer da Matriz)*
- [ ] Nuvem
- [ ] Ambiente híbrido

**2. Quais localidades, filiais ou datacenters serão monitorados?**
A Matriz em Parnamirim/RN e a Filial instalada no Setor Sudoeste em Brasília/DF.

**3. Existem dispositivos remotos que também deverão ser monitorados?**
Sim. O controlador de domínio (AD DS) e o Firewall de borda da Filial, acessados de forma segura através da VPN WireGuard.

**4. Onde serão armazenados os dados históricos de monitoramento?**
Em um banco de dados local acoplado ao servidor Zabbix no ambiente Portainer da Matriz.

**5. O monitoramento abrangerá somente a rede interna ou também serviços externos?**
Abrangerá a rede interna (LAN Matriz/Filial) e também a Zona Desmilitarizada (DMZ), onde fica hospedado o site de acesso público institucional.

---

### When (Quando?)

**1. Quando o sistema deverá ser implantado?**
Durante o ciclo de desenvolvimento do Projeto Integrador de 2026.

**2. Existe uma janela de manutenção prevista?**
Sim, deverá ser programada para fora do horário comercial de atendimento das Unidades Básicas de Saúde (madrugadas ou finais de semana).

**3. Quando os testes de monitoramento deverão ser realizados?**
Após o fechamento dos túneis VPN e da configuração de liberação de portas (TCP 10050/10051) no firewall iptables.

**4. Quando o ambiente entrará em produção?**
Simultaneamente à entrega e defesa do projeto para a banca do IFRN.

**5. Existe cronograma para inclusão gradual de ativos?**
Inicialmente os gateways e servidores essenciais (AD, DMZ, Banco). Posteriormente, pode-se incluir switches gerenciáveis e as máquinas clientes (hosts) dos consultórios médicos.

---

### Who (Quem?)

**1. Quem será responsável pela administração da ferramenta?**
A equipe de tecnologia da VoxCloud (NOC/Diretoria Técnica).

**2. Quem será responsável pela análise dos alertas?**
O setor de Suporte e Infraestrutura (Mapeado no AD DS no grupo "Suporte").

**3. Quem receberá notificações de falhas?**
Os Administradores de Sistemas e o Diretor de Operações (CTO).

**4. Quem realizará a homologação da solução?**
A diretoria do projeto em conjunto com o orientador Credson Isaac.

**5. Quem aprovará os dashboards e relatórios?**
A Diretoria Estratégica, garantindo que as métricas atendam às expectativas de nível de serviço (SLA).

**6. Quem decidirá a ferramenta a ser utilizada?**

- [x] Administradores de Sistemas
- [x] Equipe de Infraestrutura
- [ ] NOC
- [ ] Gestão de TI
- [ ] Outro

---

### How (Como?)

**1. Como os ativos serão cadastrados no sistema?**
Via interface web do Zabbix, vinculando IPs estáticos aos respectivos *templates* (ex: `Linux by Zabbix agent` e `Windows by Zabbix agent`).

**2. Como será realizado o monitoramento?**

- [x] Agentes (Zabbix Agent instalado ativamente nos hosts)
- [ ] SNMP
- [x] ICMP (Ping)
- [ ] APIs
- [ ] Scripts personalizados

**3. Como os alertas serão enviados?**

- [x] E-mail
- [ ] SMS
- [ ] Microsoft Teams
- [x] Telegram *(Ferramenta comum em NOCs para agilidade)*
- [x] WhatsApp
- [ ] Outro

**4. Como será realizado o backup da configuração?**
Através de snapshots dos contêineres no Portainer e backup agendado do banco de dados do Zabbix.

**5. Como serão gerenciadas as permissões dos usuários?**
Centralizadas de preferência com o Active Directory (AD DS), utilizando a árvore de grupos de segurança estruturada na Unidade Organizacional (OU).

**6. Como serão gerados relatórios de disponibilidade?**
Através da própria interface do Zabbix (SLA reports) e exportação visual integrada nos painéis do Grafana.

**7. Como será realizado o monitoramento de aplicações críticas?**
O site na DMZ terá portas (80/443) e processos web verificados pelo Zabbix agent, enquanto a comunicação com o banco de dados será aferida continuamente.

**8. Como será feita a retenção dos dados históricos?**
Via recurso de *Housekeeping* do Zabbix, ajustado para manter históricos recentes granulares e consolidar dados antigos, preservando armazenamento do HD de 4TB WD Purple.

**9. Como serão realizadas as atualizações e manutenções da ferramenta?**
Sendo conteinerizado via Portainer, as atualizações serão feitas recriando o contêiner com as imagens mais recentes (ex: tag *latest*), garantindo facilidade de *rollback*.

**10. Como será garantida a alta disponibilidade do sistema de monitoramento?**
Pela infraestrutura do host Docker estar blindada por trás do firewall e com nobreaks dimensionados para manter os ativos principais ligados em caso de queda de energia na Matriz.

---

### Perguntas Técnicas Complementares / Escopo

**1. Quantos servidores serão monitorados inicialmente?**
Cerca de 6 ativos principais (AD-Matriz, DMZ-Site, Host Docker, Firewall Matriz, Firewall Filial e Servidor PEC).

**2. Existe previsão de crescimento do ambiente?**
Sim. A topologia em estrela e a virtualização em contêineres/AD escalável permitem fácil expansão de novas máquinas e até de novas filiais.

**3. Quantos dispositivos de rede deverão ser monitorados?**
Inicialmente os switches core (ex: 16p Giga / 18p Fast POE Intelbras) e APs, além dos roteadores de borda/firewalls.

**4. Será necessário monitorar serviços em nuvem (Azure, AWS, Google Cloud)?**
Não no momento. A infraestrutura foca na retenção interna dos dados (On-Premises) para conformidade rígida de controle físico por exigência governamental de prontuários.

---

### Alertas e Notificações

**1. Qual deverá ser o horário de funcionamento do monitoramento?**
Monitoramento 24x7 ininterrupto.

**2. Haverá escalonamento automático de incidentes?**
Sim. Os gatilhos (triggers) identificarão falhas críticas (ex: perda de comunicação da VPN) e enviarão notificações imediatas.

**3. Qual o tempo máximo aceitável para identificação de falhas?**
Identificação imediata (tempo de checagem do agente, em média entre 1 e 5 minutos).

---

### Segurança

**1. Quem terá acesso administrativo ao sistema?**
Membros dos grupos de `Administração` e `Suporte` definidos no Active Directory da VoxCloud.

**2. Haverá integração com LDAP ou Active Directory?**
Sim, para autenticação centralizada e unificada dos administradores.

**3. Será utilizada autenticação multifator (MFA)?**
Altamente recomendável, principalmente para acessos administrativos originados da VPN.

**4. Os acessos serão auditados?**
Sim. Todos os logs do Zabbix, Nginx, AD e Firewall serão exportados, retidos e analisados pelo servidor centralizado Graylog.

---

### Relatórios

**1. Quais relatórios deverão ser gerados?**
Relatórios de estabilidade de links (WAN/VPN), saúde do hardware (CPU/Disco) e disponibilidade dos serviços (Web e Banco).

**2. Com qual frequência?**

- [ ] Diário
- [ ] Semanal
- [x] Mensal *(Reuniões de gestão)*
- [x] Sob demanda *(Em caso de incidentes ou auditorias)*

**3. Quais indicadores devem aparecer nos relatórios?**
Porcentagem de uptime (ex: 99.9%), uso médio de banda (WireGuard Transfer), incidentes registrados/resolvidos e status dos triggers.

---

### Pergunta Adicional sobre a Ferramenta / Critérios

**1. Qual sistema de monitoramento será adotado pela organização?**

- [x] Zabbix
- [ ] Nagios
- [ ] Icinga
- [ ] LibreNMS
- [ ] Outra: __________
- [ ] A definir após análise técnica dos administradores de sistemas

**Critérios para Escolha:**

- [ ] Facilidade de instalação e administração.
- [ ] Escalabilidade.
- [x] Recursos de monitoramento nativos.
- [x] Dashboards e visualização de dados *(Integração fluida com Grafana)*.
- [x] Integração com LDAP/Active Directory.
- [ ] Integração com sistemas de chamados.
- [x] Recursos de alertas e notificações.
- [x] Suporte a alta disponibilidade.
- [x] Custos de implantação e manutenção *(Plataforma Open Source)*.
- [ ] Curva de aprendizado da equipe.
