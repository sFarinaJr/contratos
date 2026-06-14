# Gerador de Contratos de Locação — Consultórios Odontológicos

Aplicação web (uma página) que gera os contratos de locação das clínicas
**Odonto Conceitualle** e **Face Instituto Odontológico**, preenchendo um modelo `.docx`
com os dados informados e salvando o resultado (DOCX + PDF) no Google Drive.

## Como funciona

```
Navegador (index.html)
  1. Você escolhe a clínica e preenche locatários, fiadores e dados do contrato.
  2. O modelo .docx é baixado e PREENCHIDO no próprio navegador (docxtemplater):
     loops por locatário/fiador, anuência de cônjuge condicional, etc.
  3. Você pode baixar o .docx na hora (botão "Baixar contrato").
        │
        ▼  envia o .docx já preenchido
Google Apps Script (web app)
  4. Converte o .docx em PDF, salva os dois no Drive, registra na planilha e envia e-mail.
```

O preenchimento dinâmico é feito por [docxtemplater](https://docxtemplater.com/) **no navegador**,
o que torna as tags robustas (não quebram entre formatações) e os blocos repetíveis confiáveis.

## Arquivos
| Arquivo | Função |
|---------|--------|
| `index.html` | A aplicação inteira (interface + lógica). |
| `clinica.docx` | Modelo do contrato da **Conceitualle**. Editável — veja `TEMPLATE-TAGS.md`. |
| `face.docx` | Modelo do contrato da **Face**. Editável — veja `TEMPLATE-TAGS.md`. |
| `imagem1.jpg` / `imagem2.jpg` | Imagens das clínicas na tela de seleção. |
| `TEMPLATE-TAGS.md` | Guia para editar os modelos sem quebrar as partes automáticas. |

## Editar o texto dos contratos
Abra `clinica.docx` ou `face.docx` no Word/Google Docs e edite à vontade — **sem alterar as tags
`{{ ... }}`**. Detalhes e lista de tags em **`TEMPLATE-TAGS.md`**. Depois de salvar, basta publicar
(o site sempre busca a versão mais recente do `.docx`).

## Publicar na web (GitHub Pages)
1. Faça o commit/push de `index.html`, `clinica.docx`, `face.docx` e as imagens.
2. Em **Settings → Pages**, selecione a branch `main` (pasta raiz).
3. Acesse a URL gerada. Pronto.

## Configuração do servidor (uma vez)
No topo do `<script>` em `index.html`:
- `APPS_SCRIPT_URL` — URL do Google Apps Script que gera o PDF e salva no Drive.
- `MAIL_TO` — e-mail que recebe a notificação com o PDF anexo.

> O Apps Script recebe o `.docx` **já preenchido**; ele apenas converte em PDF, salva no Drive,
> registra na planilha e envia o e-mail. Não precisa conhecer as tags do modelo.

## Observações
- Documentos jurídicos de casos reais (notificações, ofícios) **não** ficam neste repositório
  (são dados pessoais) — veja `.gitignore`.
- Cópias de segurança dos modelos antigos ficam como `*.docx.bak*` (locais, fora do Git).
