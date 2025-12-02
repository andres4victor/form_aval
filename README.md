# Avaliação de Comunicação – PDI

Este projeto é um formulário customizado em HTML/CSS/JS para coletar feedback sobre comunicação, integrado a um Google Forms (que armazena as respostas em uma planilha) e publicado via GitHub Pages.

---

## 1. Objetivo

- Coletar feedback estruturado de pares e gestor sobre:
  - Clareza e estruturação de argumentos  
  - Habilidade de conduzir conversas difíceis e gerenciar conflitos  
  - Empatia e escuta ativa  
- Usar NPS simples para medir a probabilidade de recomendação.
- Registrar comentários qualitativos em campo aberto.
- Centralizar as respostas diretamente em um **Google Forms + Google Sheets**, sem backend próprio.

---

## 2. Arquitetura da Solução

- **Front-end:**  
  - Arquivo HTML único (`Avaliação.html`) com:
    - Layout responsivo (HTML + CSS)
    - 3 perguntas em escala Likert (1–5)
    - Pergunta de NPS (0–10)
    - Campo de comentários abertos
    - Mensagem de sucesso pós-envio

- **Coleta de dados:**  
  - JavaScript captura as respostas do formulário.
  - Os dados são enviados via `fetch` para o endpoint de **formResponse** do Google Forms usando `URLSearchParams` e `mode: 'no-cors'`. [web:44][web:31][web:43]  

- **Publicação:**  
  - Arquivo hospedado em repositório público GitHub.
  - Servido via **GitHub Pages** com URL pública. [web:28][web:29]  

---

## 3. Estrutura do Formulário (HTML)

Elementos principais:

- Título: `Avaliação de Comunicação – PDI`
- Perguntas:
  1. Clareza e estruturação dos argumentos  
  2. Habilidade em conduzir conversas difíceis e gerenciar conflitos  
  3. Empatia e escuta ativa nas interações  
  4. Pergunta NPS: “Qual a probabilidade de você recomendar a evolução dessa pessoa para outras oportunidades?”  
  5. Campo aberto: “Comentários adicionais (opcional)”  

- Layout:
  - Escala Likert e NPS com botões estilizados (radio escondido + label clicável).
  - Mensagem de sucesso exibida após envio.

---

## 4. Integração com Google Forms

### 4.1. Configuração do Google Forms

1. Criar o formulário no Google Forms com as mesmas perguntas.
2. Para obter o **FORM_URL**:
   - Copiar o link público do formulário.
   - Substituir `viewform` por `formResponse`. [web:44]  
   - Exemplo:
     - URL pública:  
       `https://docs.google.com/forms/d/e/SEU_ID/viewform`
     - Endpoint de envio:  
       `https://docs.google.com/forms/d/e/SEU_ID/formResponse`

3. Obter os **entry IDs**:
   - Usar o recurso de **link pré-preenchido** do Google Forms.
   - Preencher valores fictícios, gerar o link e copiar.
   - Os parâmetros `entry.xxxxxxxx` no link indicam o ID de cada pergunta. [web:44]  

### 4.2. Mapeamento dos IDs utilizados

Com base no link pré-preenchido, foram definidos:

- `entry.702406475` → Pergunta 1: Clareza  
- `entry.1251980919` → Pergunta 2: Conflitos  
- `entry.1595722017` → Pergunta 3: Empatia  
- `entry.1852710350` → Pergunta 4: NPS  
- `entry.1777093880` → Campo aberto: Comentários  

### 4.3. Envio via JavaScript

Fluxo do script:

1. No `submit` do formulário:
   - `preventDefault()` para evitar recarregar a página.
   - Coleta dos valores selecionados:
     - `clarity`, `conflict`, `empathy`, `nps`, `feedback`.
2. Criação de `URLSearchParams` com o mapeamento `entry.ID → valor`. [web:31]  
3. Envio usando `fetch`:

   - Método: `POST`
   - URL: `FORM_URL` (`.../formResponse`)
   - Corpo: `formParams` (URL encoded)
   - `mode: 'no-cors'` para permitir envio ao domínio Google sem bloquear a requisição. [web:43]  

4. Em caso de sucesso (ou ausência de erro de rede):
   - Exibe mensagem de sucesso.
   - Limpa o formulário após alguns segundos.

> Observação: Com `no-cors`, o navegador não retorna status detalhado, mas o Google Forms registra a resposta normalmente. [web:43][web:44]  

---

## 5. Publicação no GitHub Pages

### 5.1. Repositório

- Usuário GitHub: `andres4victor`  
- Repositório: `form_aval`  
- Arquivo principal: `Avaliação.html`

### 5.2. Configuração do GitHub Pages

Passos gerais: [web:28][web:29]  

1. Acessar o repositório no GitHub.
2. Ir em **Settings** → **Pages**.
3. Em **Source**:
   - Selecionar branch `main`.
   - Selecionar pasta `/ (root)`.
4. Salvar.
5. Após alguns minutos, o GitHub gera a URL pública:

   - Exemplo de URL direta do arquivo:  
     `https://andres4victor.github.io/form_aval/Avalia%C3%A7%C3%A3o.html`

### 5.3. (Opcional) URL mais limpa

- Renomear o arquivo `Avaliação.html` para `index.html`.
- Acessar via:  
  `https://andres4victor.github.io/form_aval/`

---

## 6. Uso no Dia a Dia

### 6.1. Link para compartilhar com pares

- Enviar por e-mail ou Teams o link do GitHub Pages.
- Explicar em uma frase:
  - Que o formulário é anônimo (se assim for o combinado).
  - Que leva cerca de 2 minutos para responder.
  - Que faz parte do PDI focado em comunicação.

### 6.2. Acompanhamento das respostas

- Acessar o Google Forms vinculado.
- Ver respostas na aba **Respostas** ou direto na planilha Google Sheets.
- Utilizar gráficos do próprio Forms ou criar análises personalizadas na planilha.

---

## 7. Possíveis Evoluções

- Adicionar campo para identificar se o respondente é gestor, par ou outro.
- Versionar melhorias do formulário no GitHub.
- Criar painel em Google Data Studio/Looker Studio conectado à planilha.
- Automatizar lembretes via e-mail usando Apps Script ou ferramentas externas.

---

## 8. Referências

- GitHub Pages – documentação oficial. [web:28][web:29]  
- Envio de dados para Google Forms via `formResponse`. [web:44]  
- Uso de `URLSearchParams` e `fetch` para POST. [web:31][web:43]  

