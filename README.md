# Relatório do trabalho — Integração de Sistemas de Informação  
**ETL de dados de carros de drift com KNIME**  
**Carlos Filipe Santos Brandão - 26025**  
**LESI-PL**  

Outubro de 202

---

## Índice
1. [Enquadramento](#1-enquadramento)  
2. [Problema](#2-problema)  
3. [Estratégia utilizada](#3-estratégia-utilizada)  
4. [Transformações](#4-transformações)  
5. [Vídeo com demonstração](#5-vídeo-com-demonstração)  
6. [Conclusão](#6-conclusão)  
7. [Referências bibliográficas e web](#7-referências-bibliográficas-e-web)  

---

## 1. Enquadramento
- **Equipa:** Trabalho Individual de Carlos Filipe Santos Brandão  
- **Unidade Curricular:** Integração de Sistemas de Informação (ISI)  
- **Curso:** LESI-PL  
- **Breve descrição:**  
  O projeto utiliza exclusivamente a plataforma **KNIME** para implementar um processo de **ETL** que lê um ficheiro `.xlsx` com dados sobre carros de drift (Modelo, Potência, Largura dos Pneus, Estado, etc.), efetua transformações, gera ficheiros de saída em vários formatos, organiza-os em pastas, comprime num zip e envia por email.

---

## 2. Problema
**O que se pretende demonstrar / resolver:**
- Demonstrar a construção de um pipeline ETL completo usando KNIME para processamento de dados de carros de drift;
- Limpar, normalizar e transformar os dados para produzir relatórios e ficheiros JSON prontos a integrar noutros sistemas;
- Organização programática dos ficheiros de saída em pastas por critério (ex.: separação de veículos por classe/grupo, aprovação, veículos eliminados, score de relação de potência com aderência, etc.);
- Automação do empacotamento (zip) e entrega por email;
- Garantir regras de negócio aplicadas (ex.: categorização segundo potência, validação de campos).

**Motivação prática:**  
Replicar um cenário real onde um departamento técnico/operacional recolhe dados de veículos e necessita de um pipeline repetível para preparar dados para análises, partilha e integração com sistemas externos.

---

## 3. Estratégia utilizada
**Ferramenta escolhida:** KNIME Analytics Platform (workflow visual de nós)

**Fluxo geral (alto nível):**
1. Leitura do ficheiro `.xlsx` (`Excel Reader`);
2. Pré-validação e limpeza (`String Cleaner`, `Row Filter`, `Rule Engine`);
3. Ordenação e segmentação (`Sorter`, `Row Splitter`);
4. Cálculos e normalizações (`Math Formula`);
5. Agrupamentos e agregações (`GroupBy`);
6. Serialização/Export (`Table to JSON`);
7. Organização de ficheiros (`Create Folder`);
8. Compressão (`Zip Files`);
9. Envio por email (`Send Email`).

**Operadores / Nodes KNIME usados:**
1. Excel Reader  
2. String Manipulation / String Cleaner  
3. Row Filter  
4. Sorter  
5. Row Splitter  
6. Math Formula  
7. Rule Engine  
8. GroupBy  
9. Table to JSON  
10. JSON Writer  
11. Create Folder / Zip Files (nodes de arquivos)  
12. Send Email

---

## 4. Transformações
![Workflow](https://raw.githubusercontent.com/FreverZ/ISI_TP1/refs/heads/main/workflow.png)
### Explicação das transformações passo-a-passo:
**1.** É lido o ficheiro `.xlsx`;  
**2.** São criados os diretórios onde são armazenados os ficheiros;  
**3.** É passada a coluna **Condition** para maiúsculas;  
**4.** É mudado o estado dos carros de “Maintenance” para “Broken”;  
**5.** Os carros são categorizados em grupos baseados na sua potência.  
**6.** 
  **6.1:**
- 6.1.1. São separados os carros no estado **“READY”** dos restantes, sendo esses outros depois definidos como **“ELIMINATED”**;  
- 6.1.2 Os que passaram (**“READY”**) são separados em 3 categorias diferentes (**“PRO”**, **“SEMI-PRO”** e **“STREET_SPEC”**), sendo depois essa informação exportada para ficheiros `.json`;  
- 6.1.3 São também calculadas as médias de potência por grupo/classe e exportadas para um ficheiro `.xlsx`.  
 
 **6.2:**
- 6.2.1. É removido “mm” da coluna **Tire_Width** e a informação é convertida para números inteiros, permitindo assim fazer cálculos com os mesmos;  
- 6.2.2. É calculado um **score baseado na relação de potência/aderência**;  
- 6.2.3. No final, essa informação é ordenada e exportada para um ficheiro `.json`.  

**8.** Após tudo isso, é criado um diretório temporário;  
**9.** Toda a informação e ficheiros exportados são comprimidos e colocados nessa pasta temporária;  
**10.** No fim, é enviado esse ficheiro `.zip` com todos os dados para o email.  


---

## 5. Vídeo com demonstração
[Vídeo de demonstração (Google Drive)](https://drive.google.com/file/d/1bg8V9_Gfi7Q63Yp2i3z5SZfFc_v8oEs4/view?usp=sharing)

---

## 6. Conclusão
O workflow em KNIME demonstrou a viabilidade de construir um pipeline ETL robusto e repetível para dados de carros de drift. O processo cobre a leitura de ficheiros Excel, limpeza, validação, segmentação, cálculo de métricas, escrita em vários formatos, organização de ficheiros, compressão e envio por email, mantendo registos (logs) para auditoria.

**Trabalhos futuros / melhorias propostas:**
- Integração com APIs externas;
- Dashboard interativo (ex.: conectar os resultados a um painel em Grafana/Power BI/Tableau ou usar KNIME WebPortal);
- Testes unitários para as regras de transformação e pipelines (ex.: validar outputs esperados para ficheiros de input de teste);
- Operações com bases de dados SQL.

---

## 7. Referências bibliográficas e web
- Plataforma de e-learning Moodle (materiais da UC e exemplos de projetos anteriores)  
- Tutoriais do YouTube sobre criação de workflows e automações no KNIME  
- Documentação oficial do KNIME — consulta de detalhes técnicos sobre os nodes usados e boas práticas de ETL: <https://docs.knime.com>
