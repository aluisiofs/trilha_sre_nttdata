# Governança e Fundamentos Operacionais de SRE

**Organização:** NTT DATA – Engineering & Reliability  
**Autor:** Analista de Engenharia de Sistemas SRE Júnior  
**Versão:** 1.0  
**Data:** 2026-11-01  

---

## 1. Síntese Filosófica de SRE

O Site Reliability Engineering (SRE) é a disciplina que surge quando se exige que engenheiros de software projetem e operem infraestrutura de grande escala [1,13]. A premissa fundamental do SRE estsabelece que as operações de TI devem ser tratadas estritamente como problemas dse engenharia de software [1,2]. Na célebre metáfora de orientação a objetos, se o DevOps atua como uma interface cultural focada na quebra de silos e colaboração, o SRE é a classe concreta quew implementa essa interface ('class SRE implemets interface DevOps'), fornecendo regras matematicamente acionaveis para governar o risco, a disponibilidade e a velocidade de implementação [2,14].

## 2. Taxonomia Comparativa dos Modelos Operacionais

| Dimensão | SysAdmin Tradicional | Cultura DevOps | Site Reliability Engineering (SRE) |
| :--- | :--- | :--- | :--- |
| **Abordagem** | Reativa, manual, focada em tickets | Cultural, quebra de silos, integração CI/CD | De engenharia, codificação e governança quantitativa |
| **Perfil** | Administrador de Infraestrutura | Híbrido (Automação de CI/CD) | ~50% Dev + ~50% Engenharia de Sistemas |
| **Métrica de Sucesso** | Tentativa de Uptime de 100% | Frequência de Deploy e Lead Time | Cumprimento de SLOs e Gestão do Error Budget |
| **Tratamento de Falha** | Culpa individual, mitigação pontual | Aprendizado coletivo | Blameless Postmortem obrigatório |

## 3. Governança do Trabalho Operacional e Eliminação de Toil

### 3.1 Definição e Mapeamento de Toil

- **Exemplo 1 (Manual):** Reset manual de conexões de banco de dados presas.
- **Exemplo 2 (Repetitivo):** Limpeza manual de logs de disco cheios em servidores nativos.
- **Exemplo 3 (Sem Valor Duradouro):** Aprovação manual de deploys em homologação por e-mail.

### 3.2 Regras de Governança

- **Teto Operacional de 50%:** Máximo de 50% do tempo dedicado a tarefas manuais, tickets e plantão; 50% obrigatoriamente alocados em desenvolvimento de software e automação.
- **Válvula de Segurança:** Se o Toil ultrapassar 50%, a operação e o sobreaviso retornam automaticamente para a equipe de desenvolvimento.
- **Limites de Sobreaviso:** No máximo 2 alertas críticos (pages) por turno de 8–12 horas.

## 4. Estudo de Caso de Aplicação Real

### Mapeamento do Ecossistema: Gateway de Pagamentos Crítico

Em uma plataforma de comércio eletrônico, o serviço de **Gateway de Pagamentos** é uma Jornada Crítica do Usuário (CUJ) [15]. O papel do SRE no ciclo de vida deste sistema abrange:

1. **Engajamento Precoce (Early Engagement):** Participação no design da arquitetura antes do código ir para produção, exigindo padrões de resiliência como *timeouts*, *retries* com *backoff* exponencial e *circuit breakers* [16-18].

2. **Production Readiness Review (PRR):** Auditoria técnica rigorosa para validar se a aplicação possui métricas expostas, *dashboards* configurados e alertas baseados no comportamento do usuário antes de receber tráfego real [16, 19, 20].

3. **Operação Baseada em SLOs:** Garantir que deploys de novas regras de antifraude ocorram apenas enquanto houver saldo no *Error Budget*, preservando a estabilidade e a receita do negócio [21-23].

## 5. Protocolo de Pós-Mortem Sem Culpa (Blameless Postmortem)

### 5.1 Resumo do Incidente (Simulado)

- **Data/Hora:** 2026-11-01 14:00 UTC
- **Impacto:** Indisponibilidade de 15 minutos na API de Checkout.
- **Causa Raiz Sistêmica:** Falha de limite de conexões no pool após deploy sem teste de carga em staging.

### 5.2 Ações Corretivas e Preventivas (Engenharia)

- [ ] Implementar Circuit Breaker na camada de persistência.
- [ ] Adicionar automação de teste de resiliência no pipeline CI/CD.