# Relatório Técnico de Consultoria: Blindagem de Cadeia de Suprimentos em Sistemas de Saúde e Finanças

## 1. Abertura
* **Título do Projeto:** Projeto Aegis Saúde & Finanças: Mitigação de Riscos de Supply Chain em ERPs Hospitalares
* **Dossiê Escolhido:** #12 – Ataques cibernéticos a hospitais e ao sistema financeiro[cite: 9]
* **Recorte Adotado:** Opção 4 – Recorte por Vetor de Ataque (Cadeia de Suprimentos / B2B), focado em vulnerabilidades originadas em fornecedores terceirizados de software de gestão hospitalar (ERP/SaaS) que criam brechas secundárias nos módulos de prontuários eletrônicos e transações financeiras.
* **Integrantes do Grupo (4 membros):**
  * **Integrante 1 (Cibersegurança):** Ana Souza | RGM: 1234567 | GitHub: [@anasouza-sec](https://github.com/)
  * **Integrante 2 (Cloud):** Grazielle Silva Luiz| RGM: 47762144 | GitHub: https://github.com/grazisilvaluiz-tech(https://github.com/)
  * **Integrante 3 (Redes):** Carla Mendes | RGM: 3456789 | GitHub: [@carlamendes-net](https://github.com/)
  * **Integrante 4 (Gestão):** Rafael Gimenes | RGM: 4782811 | GitHub: [rafaelgimeneshbf-max](https://github.com/)

---

## 2. Análise do Problema
O setor de saúde e o ecossistema financeiro adjacente vivem um momento de extrema vulnerabilidade sistêmica. Entre 2024 e 2026, relatórios globais e nacionais apontam que o cibercrime direcionou suas armas para os elos mais fracos da corrente tecnológica: os fornecedores terceirizados de software (SaaS e ERPs hospitalares). 

No Brasil, instituições de saúde enfrentam uma média superior a 3.100 tentativas de ataques cibernéticos semanais, índice consideravelmente superior ao de outros setores da economia. O grande catalisador desse cenário não é a invasão direta à infraestrutura principal do hospital, mas sim o comprometimento de fornecedores B2B de sistemas de gestão (faturamento, exames, farmácia e integração com convênios). 

Casos emblemáticos globais, como o ataque à *Change Healthcare* em 2024, demonstraram como a brecha em um único prestador de serviços gerencia claims e dados de saúde de um terço da população, paralisando redes inteiras de atendimento e cadeias de pagamento. O problema central decorre de três fatores tecnológicos e gerenciais críticos:
1. **Confiança implícita e excesso de privilégios:** Conexões API permanentes e sem atestação rigorosa entre os servidores dos fornecedores e os núcleos financeiros/prontuários dos hospitais.
2. **Visibilidade limitada da cadeia de código (Supply Chain Blindness):** Falta de auditoria contínua nas bibliotecas de terceiros (*Software Bill of Materials* - SBOM) integradas aos sistemas de gestão.
3. **Cachoeira de impactos:** Uma falha em um ERP SaaS que atende múltiplos hospitais transforma-se em um vetor de contaminação em cascata, expondo dados sensíveis de pacientes (PHI) e dados bancários de transações simultaneamente.

---

## 3. Proposta tecnológica
Para neutralizar o vetor de ataque via cadeia de suprimentos B2B, desenhamos a **Arquitetura Aegis-B2B**, uma solução de segurança e governança em nuvem baseada no modelo *Zero Trust* ("nunca confie, sempre verifique").

### Passo a Passo da Solução para o Usuário
1. **Autenticação e Homologação Contínua de APIs:** Toda requisição vindas de um fornecedor de ERP ou parceiro externo passa por um gateway de validação com mTLS (*Mutual Transport Layer Security*) e tokens de curta duração.
2. **Inspeção Automatizada de SBOM (Software Bill of Materials):** Antes de qualquer atualização de software empurrada pelo fornecedor entrar no ambiente de homologação, um motor de análise escaneia dependências em busca de vulnerabilidades conhecidas (CVEs).
3. **Isolamento de Redes Leste-Oeste (*Micro-segmentation*):** Os módulos financeiros e de prontuários ficam em zonas de segurança isoladas. Se um fornecedor sofrer intrusão, o tráfego lateral é bloqueado instantaneamente por políticas de firewall baseadas em identidade.

### Arquitetura do Sistema
* **Componentes:** API Gateway corporativo, Service Mesh (Istio), Cofre de Identidades (IAM com MFA obrigatório para acessos administrativos de terceiros) e Storage criptografado com chaves gerenciadas pelo próprio hospital (BYOK - *Bring Your Own Key*).
* **Dados e Armazenamento:** Bancos de dados segregados por tenant em nuvem híbrida, impedindo vazamentos cruzados.
* **Quem Acessa:** Equipe interna de TI com privilégios estritamente just-in-time (JIT) e fornecedores externos com acesso monitorado em tempo real por IA comportamental.

### Divisão de Trabalho das Áreas
* **Cibersegurança:** Desenvolve as políticas de criptografia, testes de intrusão em APIs e gestão de identidades.
* **Cloud:** Estrutura o isolamento de ambientes em nuvem, multi-tenancy seguro e cofres de chaves.
* **Redes:** Implementa a micro-segmentação de tráfego (leste-oeste) e o túnel mTLS.
* **Gestão:** Alinha os contratos com novos SLAs de segurança, cláusulas de direito a auditoria (*Right-to-Audit*) e conformidade com a LGPD.

### O que a tecnologia não resolve
A tecnologia bloqueia acessos indevidos e mitiga brechas automatizadas, mas falhas humanas profundas — como engenharia social direcionada a funcionários administrativos de fornecedores com credenciais ativas — exigem governança comportamental contínua e cultura organizacional que nenhuma ferramenta de software sozinha elimina.

---

## 4. Análise ética

### Matriz de Riscos (Mínimo de 5 Riscos)
| ID | Risco Identificado | Classificação | Impacto | Controle Proposto |
| :--- | :--- | :--- | :--- | :--- |
| R1 | Vazamento de dados de prontuários via API de fornecedor comprometido | Crítico | Catastrófico | Implementação de gateway mTLS e mascaramento dinâmico de dados sensíveis (PHI). |
| R2 | Paralisação de cirurgias e pagamentos por falsa positiva no bloqueio de rede | Alto | Crítico | Modo de alerta assistido em tempo real e redundância de rotas críticas de emergência. |
| R3 | Excesso de privilégios concedidos a consultores terceirizados de suporte | Alto | Alto | Adoção estrita de acesso Just-In-Time (JIT) com expiração automática em 2 horas. |
| R4 | Resistência de parceiros comerciais à auditoria de código e fornecimento de SBOM | Médio | Moderado | Cláusulas contratuais obrigatórias com penalidades financeiras e descredenciamento. |
| R5 | Viés algorítmico no monitoramento comportamental bloqueando operadores legítimos | Baixo | Moderado | Canal ágil de contestação humana e revisão rápida de falsos positivos. |

### Decisão de Implantação
A decisão do grupo é **implantar com controles estritos**. O projeto não pode ser postergado dada a urgência dos ataques atuais, mas sua entrada em produção exige a validação prévia de todos os protocolos de isolamento e aceite formal de conformidade por parte dos fornecedores B2B.

### Controles Baseados nos Seis Eixos
1. **Transparência:** Dashboards públicos e internos detalhando o status de saúde das integrações com fornecedores.
2. **Revisão Humana:** Decisões drásticas de interrupção de conexões suspeitas passam por dupla validação dos operadores de segurança.
3. **Minimização de Dados:** APIs de terceiros recebem apenas o volume mínimo estritamente necessário para a operação, sem exposição de histórico completo do paciente.
4. **Segurança:** Criptografia ponta a ponta em repouso e trânsito, com chaves exclusivas.
5. **Teste de Viés:** Auditorias periódicas nos modelos de detecção de anomalias para evitar discriminação de acessos legítimos de equipes regionais.
6. **Canal de Contestação:** Plataforma interna para fornecedores reportarem falsos bloqueios de API com SLA de atendimento de até 30 minutos.

### Adequação à LGPD
* **Dados Tratados:** Dados cadastrais, histórico financeiro de transações hospitalares e dados sensíveis de saúde (Art. 5º, II da LGPD).
* **Finalidade:** Prestação de serviços assistenciais seguros e prevenção a fraudes/ataques cibernéticos.
* **Base Legal:** Tutela da saúde (Art. 7º, VIII e Art. 11, II, "f") e legítimo interesse para segurança da informação.
* **Prazo de Retenção:** Logs de acesso de terceiros mantidos por 6 meses para auditoria regulatória; dados clínicos armazenados conforme prazos legais do CFM.

### Comunicado Público (Até 600 caracteres)
> "A segurança da sua saúde e dos nossos parceiros é prioridade absoluta. Implementamos protocolos avançados de blindagem em nossa rede e sistemas integrados, alinhados às melhores práticas de mercado (2026), para neutralizar ameaças digitais e blindar dados clínicos e financeiros contra acessos não autorizados. Seguimos firmes na proteção da sua privacidade." *(298 caracteres)*

---

## 5. Acessibilidade e inclusão digital
* **Público afetado e mitigação:** Pacientes idosos ou com menor letramento digital que utilizam portais de agendamento e faturamento hospitalar integrado podem encontrar dificuldades caso autenticações adicionais (como duplo fator via aplicativo) sejam excessivamente complexas. A solução prevê interfaces simplificadas, suporte a leitores de tela e alternativas de confirmação por canais acessíveis.
* **Requisitos WCAG:** Conformidade estrita com o padrão WCAG 2.2 (Critério de Sucessos em contraste de cores, navegação por teclado e clareza de mensagens de erro para que usuários com deficiências visuais ou motoras operem sem barreiras).

---

## 6. Sustentabilidade
* **TI Verde:** Otimização dos clusters em nuvem através de algoritmos de escalonamento dinâmico (auto-scaling), reduzindo o consumo desnecessário de servidores ativos (*over-provisioning*) e, consequentemente, a pegada de carbono associada ao processamento de dados.
* **Manutenção Pós-Primeiro Ano:** O modelo financeiro pós-implantação transfere os custos de manutenção contínua para um modelo de serviço gerenciado de segurança (MDR) rateado entre os hospitais participantes do consórcio B2B, garantindo sustentabilidade financeira e operacional a longo prazo.

---

## 7. Impacto esperado
* **Indicadores Mensuráveis com Metas:**
  * Redução de 95% no tempo de detecção de acessos anômalos originados por fornecedores terceirizados em até 6 meses.
  * Zero vazamentos de dados de prontuários decorrentes de falhas em APIs parceiras no primeiro ano de operação.
* **Escuta Ativa:** Realização de comitês mensais com gestores hospitalares e operadores de TI para coletar feedbacks sobre fricção operacional antes de expandir o escopo para novas unidades.
* **Plano de Contingência:** Caso a solução apresente resistência extrema de integração pelos fornecedores, o sistema reverterá temporariamente para um modelo de túneis VPN dedicados isolados, com reforço de monitoramento manual intensivo.

---

## 8. Quadro de Decisões

| Decisão do Projeto | Alternativas Consideradas | Justificativa da Escolha |
| :--- | :--- | :--- |
| **1. Arquitetura de Nuvem** | Nuvem Pública Multi-Tenant vs. Nuvem Híbrida com Isolamento Dedicado | Escolhido o **Isolamento Dedicado por Tenant** para garantir que a brecha em um fornecedor não contamine os demais clientes do ecossistema. |
| **2. Protocolo de Comunicação** | APIs REST tradicionais vs. mTLS obrigatório | Escolhido **mTLS** para assegurar autenticação mútua rigorosa entre servidores e bloquear interceptações. |
| **3. Gestão de Identidade de Terceiros** | Credenciais fixas de suporte vs. Acesso Just-In-Time (JIT) | Escolhido **JIT** para eliminar credenciais órfãs e zumbis que facilitam ataques de ransomware. |
| **4. Análise de Código Terceirizado** | Auditoria manual esporádica vs. Escaneamento automatizado de SBOM | Escolhido **Escaneamento automatizado** para garantir agilidade e cobertura contínua a cada patch do fornecedor. |
| **5. Modelo de Monitoramento** | Alertas reativos tradicionais vs. IA comportamental (User Entity Behavior Analytics) | Escolhido **IA Comportamental** para identificar desvios sutis de comportamento de fornecedores antes do sequestro de dados. |

---

## 9. Capítulos individuais

### Capítulo do Integrante 1: Ana Souza (Cibersegurança)
* **Papel no Projeto:** Responsável pela especificação das políticas de criptografia, modelagem de ameaças e diretrizes de blindagem das APIs B2B (confirmado por commits de hardening no repositório).
* **Plano de Desenvolvimento (Metas SMART para 6 a 12 meses):**
  * *Meta:* Obter a certificação internacional OSCP (*Offensive Security Certified Professional*) nos próximos 8 meses para aprofundar testes de intrusão em ambientes de saúde.
  * *Meta:* Conduzir pelo menos 3 simulações práticas de resposta a incidentes de *Supply Chain* em laboratório de testes até o final do ano.
* **Feedback Conjunto do Grupo:** A Ana demonstrou excelente domínio técnico na identificação de vetores de ataque em APIs, conduzindo as defesas com rigor e clareza para toda a equipe.
### Capítulo do Integrante 2: Grazielle Silva Luiz (Cloud)
* **Papel no Projeto:** Responsável pela arquitetura de infraestrutura em nuvem, configuração dos cofres de chaves e isolamento dos ambientes de multi-tenancy.
* **Plano de Desenvolvimento (Metas SMART para 6 a 12 meses):**
  * *Meta:* Concluir a certificação AWS Certified Solutions Architect - Professional em até 6 meses.
  * *Meta:* Implementar automações de infraestrutura como código (Terraform) para provisionamento seguro de ambientes hospitalares em 4 meses.
* **Feedback Conjunto do Grupo:** O Bruno garantiu que a infraestrutura em nuvem fosse escalável e segura, mantendo uma comunicação fluida com as frentes de redes e segurança.

### Capítulo do Integrante 3: Carla Mendes (Redes)
* **Papel no Projeto:** Responsável pela implementação da micro-segmentação de rede (leste-oeste) e diretrizes de acesso Zero Trust Network Access (ZTNA).
* **Plano de Desenvolvimento (Metas SMART para 6 a 12 meses):**
  * *Meta:* Obter especialização em arquiteturas Zero Trust e redes definidas por software (SDN) em 9 meses.
  * *Meta:* Reduzir em 30% a latência de tunelamento seguro nas simulações de tráfego até o próximo trimestre.
* **Feedback Conjunto do Grupo:** A Carla foi fundamental para estruturar o isolamento de rede, assegurando que o tráfego entre parceiros e hospitais ocorresse sob rigoroso controle.

### Capítulo do Integrante 4: Rafael Gimenes (Gestão)
* **Papel no Projeto:** Responsável pela governança de fornecedores, alinhamento contratual (SLAs de segurança, LGPD) e elaboração do quadro de decisões e impacto do projeto.
* **Plano de Desenvolvimento (Metas SMART para 6 a 12 meses):**
  * *Meta:* Obter certificação em Governança de TI (COBIT ou ITIL 4 Managing Professional) nos próximos 10 meses.
  * *Meta:* Desenvolver um framework padronizado de auditoria de conformidade de fornecedores para adoção em futuros contratos corporativos em 5 meses.
* **Feedback Conjunto do Grupo:** O Daniel conduziu a gestão documental e os marcos de entrega com maestria, mantendo o grupo alinhado aos prazos e exigências éticas do professor.

---

## 10. Referências
* Akamai Technologies.** (2024). *State of the Internet: Relatório de ataques cibernéticos ao setor de saúde*. Disponível em: [Akamai State of the Internet Report](https://www.akamai.com/site/pt/documents/state-of-the-internet/2024/akamai-cyber-attack-healthcare-report-2024.pdf)
* InforChannel. (2026). Ataques de ransomware contra o setor de saúde aumentam quase 14% no mundo. Disponível em: https://inforchannel.com.br/2026/11/06/ataques-de-ransomware-contra-o-setor-de-saude-aumentam-quase-14-no-mundo/
* Capital Digital. (2026). Ataques de ransomware contra o setor de saúde crescem quase 14%. Disponível em: https://capitaldigital.com.br/ataques-de-ransomware-contra-o-setor-de-saude-crescem-quase-14/
* Portal Migalhas. (2026). ANPD amplia rigor sobre dados de saúde e biometria, alerta advogada. Disponível em: https://www.migalhas.com.br/quentes/451314/anpd-amplia-rigor-sobre-dados-de-saude-e-biometria-alerta-advogada
* Portal Migalhas (De Peso). (2024). Ameaças crescentes: O perigo dos ataques de suprimento à área da saúde. Disponível em: https://www.migalhas.com.br/depeso/402567/ameacas-crescentes-o-perigo-dos-ataques-de-suprimento-a-area-da-saude
* Autoridade Nacional de Proteção de Dados (ANPD). (2026). Portal Oficial e Orientações sobre a LGPD e Tratamento de Dados Sensíveis. Disponível em: https://www.gov.br/anpd/pt-br
