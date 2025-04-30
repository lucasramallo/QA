### ✅ **JaCoCo**  
JaCoCo é a principal ferramenta de cobertura de código para Java.  
Gera relatórios de cobertura por **linha**, **instrução** e **branch**, com fácil integração via **Maven**, **Gradle** e **SonarQube**.  
Ideal para acompanhar quanto do código é realmente exercitado pelos testes unitários.

### ☁️ **Codecov**  
Codecov é uma plataforma em nuvem que coleta e exibe relatórios de cobertura de testes.  
Integra-se com **GitHub/GitLab/CircleCI** e consome dados gerados por ferramentas como **JaCoCo**.  
Permite visualizar gráficos e métricas de cobertura diretamente nos **PRs** e no repositório.

### 🧾 Cabeçalhos do Relatório

Cada linha representa um pacote do seu projeto. As colunas significam:

| Coluna             | Significado                                                                 |
|--------------------|------------------------------------------------------------------------------|
| **Element**         | Nome do pacote ou classe                                                    |
| **Missed Instructions** | Instruções de código não executadas durante os testes               |
| **Cov.**            | Cobertura percentual dessas instruções                                     |
| **Missed Branches** | Decisões lógicas (como if/else) não cobertas                               |
| **Cov. (branches)** | Percentual de decisões cobertas                                            |
| **Missed Cxty**     | Complexidade ciclomática não coberta (fluxos lógicos distintos)            |
| **Missed Lines**    | Linhas de código não executadas                                            |
| **Missed Methods**  | Métodos não executados                                                     |
| **Missed Classes**  | Classes totalmente não testadas                                            |


### 🔬 **Pitest**  
Pitest realiza **testes de mutação**, alterando o código propositalmente para avaliar a eficácia dos testes.  
Mostra se seus testes realmente conseguem detectar **erros lógicos sutis**.  
É um forte complemento à cobertura, já que revela **falsos positivos** (testes que cobrem mas não validam).

### 🧩 **Usando JaCoCo, Codecov e Pitest juntos**  
Use **JaCoCo** para medir quanto do código é testado, **Codecov** para visualizar e rastrear a cobertura ao longo do tempo em sua pipeline, e **Pitest** para garantir que os testes não apenas cobrem o código, mas também o validam corretamente.  
Essa combinação oferece uma análise completa:  
- **Quantidade** (JaCoCo/Codecov)  
- **Qualidade** (Pitest)

Integradas em um CI (como **GitHub Actions** ou **Jenkins**), essas ferramentas tornam sua estratégia de testes mais **confiável e auditável**.
