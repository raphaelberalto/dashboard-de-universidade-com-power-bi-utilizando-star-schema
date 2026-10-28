# 📊 Desafio de Modelagem Dimensional: Análise de Professores (Star Schema)

Projeto prático de transformação de um modelo relacional transacional (OLTP) em um modelo dimensional (*Star Schema* - OLAP) voltado para a análise do corpo docente universitário.

---

## 📌 Visão Geral do Desafio

O objetivo principal deste projeto é conceber um diagrama dimensional em **esquema em estrela (*Star Schema*)** a partir de um modelo relacional fornecido como base.

### 🎯 Foco da Análise
* **Objeto de Análise:** **Professor**.
* **Escopo:** A tabela fato consolida os dados essenciais relativos ao professor, às disciplinas e cursos ministrados, às alocações departamentais e às métricas de docência.
* **Premissa:** Dados de alunos foram descartados conforme as diretrizes do desafio, mantendo o foco analítico exclusivo na perspectiva do corpo docente.
* **Tratamento Temporal:** Criação de uma dimensão de datas estruturada para possibilitar análises em múltiplos níveis de granularidade temporal (ano, semestre, bimestre, trimestre, mês e dia da semana).

---

## 🏗️ Arquitetura do Modelo Dimensional

O modelo segue rigorosamente os princípios de modelagem dimensional de Ralph Kimball, posicionando uma tabela fato central conectada diretamente a tabelas de dimensão desnormalizadas.

```
       [ Curso ]            [ Datas ]
           \                    /
            \                  /
    [ Dados Professor ] — [ Fato ] — [ Departamento ]
            /
           /
      [ Disciplina ]
```

### 1. Tabela Fato (`Fato`)
Responsável por armazenar as chaves estrangeiras que apontam para as dimensões e as métricas numéricas agregáveis da atividade docente:

* **Chaves:**
  * `id_Fato` (Chave Primária)
  * `Datas_idDatas` (FK para `Datas`)
  * `Curso_idCurso` (FK para `Curso`)
  * `Disciplina_idDisciplina` (FK para `Disciplina`)
  * `Departamento_idDepartamento` (FK para `Departamento`)
  * `Dados Professor_idDados Professor` (FK para `Dados Professor`)
* **Métricas Numéricas:**
  * `Cursos_Ministrados` (INT)
  * `Disciplinas_Ministradas` (INT)
  * `Professores_Alocados` (INT)
  * `Departamentos_Existentes` (INT)

### 2. Tabelas Dimensão

| Dimensão | Papel e Atributos |
| :--- | :--- |
| **`Dados Professor`** | Contém o perfil e os dados cadastrais do docente: `idDados Professor`, `Nome`, `CPF`, `Data de Nascimento`, `Formação` e `Estado Civil`. |
| **`Disciplina`** | Descreve as disciplinas ofertadas pelo corpo docente: `idDisciplina` e `Nome`. |
| **`Curso`** | Registra os cursos aos quais as disciplinas e professores estão vinculados: `idCurso` e `Nome`. |
| **`Departamento`** | Contexto organizacional do professor na instituição: `idDepartamento`, `Nome`, `Campus` e `Coordenador`. |
| **`Datas`** | Dimensão de calendário genérica com suporte a múltiplas granularidades: `idDatas`, `Dia`, `Mês`, `Ano`, `Dia_Semana`, `Nome_Mês`, `Semestre`, `Bimestre` e `Trimestre`. |

---
