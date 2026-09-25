#N8N
---

## 🛠️ Ferramentas e Nós do n8n Utilizados

Para construir este fluxo, você precisará utilizar 4 nós principais no n8n:

1. **Google Forms Trigger (Gatilho):**
* **Função:** Monitora o formulário em tempo real. Sempre que um novo lead preencher e enviar o Google Forms, este nó dispara a execução do workflow.


2. **If (Condicional):**
* **Função:** Aplica a regra de negócio exigida. Ele verifica se o campo de e-mail preenchido pelo lead contém uma estrutura válida (ex: presença do caractere `@` e de um domínio com ponto).


3. **Google Sheets (Ação):**
* **Função:** Conecta-se à planilha da equipe comercial e adiciona uma nova linha (`Append Row`) contendo as informações do lead (Nome, E-mail, Telefone, Data/Hora, etc.).


4. **Gmail (Ação):**
* **Função:** Envia um e-mail automatizado de confirmação/boas-vindas diretamente para a caixa de entrada do lead, garantindo um primeiro contato rápido.



---

## ⚙️ Lógica de Funcionamento do Workflow

O fluxo foi desenhado para ser executado de forma totalmente autônoma e segura. Veja o passo a passo de como os dados trafegam:

* **Passo 1: Captura (Google Forms Trigger)**
O lead preenche o formulário. O n8n recebe instantaneamente os dados via webhook configurado no Google Forms.
* **Passo 2: Validação (Nó If)**
Antes de gastar linhas na planilha ou disparar e-mails errados, o nó **If** analisa o campo de e-mail.
* *Condição:* O e-mail contém `@` e `.`?
* **Se falso (E-mail inválido):** O fluxo é encerrado imediatamente (caminho `False`), evitando registros corrompidos na base da equipe comercial.
* **Se verdadeiro (E-mail válido):** O fluxo prossegue para o próximo passo (caminho `True`).


* **Passo 3: Armazenamento (Google Sheets)**
O n8n pega os dados validados do lead e os insere na planilha oficial da equipe comercial. Isso garante que os vendedores tenham acesso imediato aos leads quentes em um local centralizado.
* **Passo 4: Comunicação (Gmail)**
Logo após salvar na planilha, o nó do **Gmail** é acionado para enviar uma mensagem personalizada ao lead (ex: *"Olá [Nome], recebemos seu contato e nossa equipe comercial retornará em breve!"*).

---

> 💡 **Dica de Especialista:** Para garantir que o e-mail seja validado com alta precisão no nó **If**, você pode utilizar uma expressão regular (Regex) simples no n8n que confere o padrão da string, evitando que entradas como "abc" passem pelo filtro.

Ficou com alguma dúvida sobre a configuração dos parâmetros de autenticação (OAuth2) dessas ferramentas ou quer ver o modelo de expressão regular para o e-mail?
